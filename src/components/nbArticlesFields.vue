<template>
  
  <div class="flex m-2 justify-center">
    <label for="nbArticles">Nombre d'articles:</label>
    <input class="w-1/6 h-5 lg:w1/4 lg:h-10"
      type="number"
      id="nbArticles"
      :value="articleCount"
      @input="updateArticleCount"
    />
  </div>
  
</template>

<script>

export default {
  props: {
    articleCount: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    updateArticleCount(event) {
      this.$emit('updateArticleCount', Number(event.target.value));
    },
  },
};

</script>
