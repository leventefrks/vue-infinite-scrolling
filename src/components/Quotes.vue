<template lang="html">
  <section
    class="min-h-screen px-4 pt-4 pb-24 flex flex-col items-center justify-center bg-gray-50 text-gray-600 space-y-4"
  >
    <div class="absolute top-6 text-center">
      <h1 class="text-3xl font-semibold tracking-wider text-purple-500">
        Vue Infinite Scrolling
      </h1>

      <h2 class="mb-3 text-xl text-purple-400">
        Quotes for infinity and beyond
      </h2>
    </div>

    <div v-if="isQuotesLoaded" class="relative top-16 max-w-3xl pb-12">
      <div
        v-for="quote in quotes"
        :key="quote._id"
        class="my-5 text-lg text-center tracking-tight"
      >
        <div>"{{ quote.quoteText }}"</div>
        <div class="text-gray-400">- {{ quote.quoteAuthor }}</div>
      </div>
    </div>

    <div v-else class="text-3xl font-semibold tracking-wider text-purple-500">
      Quotes are coming..
    </div>

    <div v-observe-visibility="observeVisibilityOptions" />
  </section>
</template>

<script>
import axios from 'axios';

const Quotes = {
  name: 'Quotes',

  data() {
    return {
      quotes: [],
      page: 1,
      totalPage: 7268,
      observeVisibilityOptions: {
        callback: this.handleInfinityScroll,
        intersection: {
          threshold: 0.9,
        },
      },
    };
  },

  mounted() {
    this.getQuotes();
  },

  computed: {
    isQuotesLoaded() {
      return this.quotes.length > 0;
    },
  },

  methods: {
    async getQuotes() {
      try {
        const response = await axios.get(
          `https://quote-garden.herokuapp.com/api/v3/quotes?page=${this.page}`
        );
        const data = await response.data;
        this.quotes.push(...data.data);
      } catch (error) {
        console.log(error);
      }
    },

    handleInfinityScroll(isVisible) {
      if (!isVisible || this.page >= this.totalPage) return;

      this.page++;
      this.getQuotes();
    },
  },
};

export default Quotes;
</script>
