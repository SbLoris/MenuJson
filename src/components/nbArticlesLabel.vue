<template>
  <div class="flex m-2 justify-center">
    <p>Nombre d'articles en temps réel: {{ articleCount || 0 }}</p>
  </div>
</template>

<script>
export default {
  props: {
    articleCount: {
      type: Number,
      required: false,
      default: 0,
    },
  },
};
</script>
