<template>
  <div>
    <label for="nbArticles">Nombre d'articles:</label>
    <input
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
