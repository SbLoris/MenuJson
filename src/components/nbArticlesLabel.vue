<template> 
  <div class="flex m-2 justify-center lg:w-1/6 md:w-2/4 sm:w-3/4">
    <p>Articles: {{ articleCount || 0 }}</p>
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
