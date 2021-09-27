<template>
  <div
    class="min-h-screen p-4 flex flex-col items-center justify-center bg-gray-100 text-gray-600 space-y-4"
  >
    <div class="absolute top-6">
      <h1 class="text-3xl">Vue Infinite Scrolling</h1>
      <h2 class="my-4 text-xl text-gray-400">Quotes for infinity and beyond</h2>
    </div>

    <template v-if="isQuotesLoaded">
      <div class="relative top-16">
        <div v-for="quote in quotes" :key="quote._id">
          <div>
            <p class="my-5 text-lg text-center">
              <span>"{{ quote.quoteText }}"</span><br />
              <span class="text-gray-400">- {{ quote.quoteAuthor }}</span>
            </p>
          </div>
        </div>
      </div>
    </template>
    <template v-else><div class="text-3xl">Quotes Are Coming..</div></template>
    <div v-observe-visibility="handleInfinityScroll" />
  </div>
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
