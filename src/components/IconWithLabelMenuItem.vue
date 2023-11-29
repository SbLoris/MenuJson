<template>
  <div class="lg:w-1/2 md:w-3/4 sm:w-full">
  <div @click="handleClick" :class="{ 'flex m-2 justify-center': true, 'opacity-50 cursor-not-allowed disabled': !permission }">
   
      <img :src="icon" alt="Icon" class="rounded w-10 h-auto max-h-full" />
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
  methods: {
   
  },
  computed: {
    iconClass() {
      return this.$options.name && this.$options.name.includes('MenuItem') ? 'home-icon' : 'default-icon';
    },
  },
};
</script>

<style scoped>
  /* Add scoped styles here if needed */
</style>
