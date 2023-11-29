<template>
    <div @click="handleClick" :class=" { 'flex m-2 justify-center': true, 'opacity-50 cursor-not-allowed disabled':!permission }">
    <div class="flex m-2 justify-center">
    <img :src="icon" alt="Icon" class="rounded w-1/5 h-auto " />
    <span>{{ label }}</span>
  </div>
</div>
</template>

<script>
export default {
  props: {
    icon: String,
    label: String,
    permission: Boolean,
    link: String,
  },

  computed: {
    iconClass() {
      return this.$options.name && this.$options.name.includes('MenuItem') ? 'home-icon' : 'default-icon';
    },
  },
};
</script>

<style scoped>


</style>
