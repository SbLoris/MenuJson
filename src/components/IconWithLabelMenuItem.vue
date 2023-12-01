<template>
  <div  @click="handleClick" :class="{ 'lg:w-1/2 md:w-3/4 sm:w-full flex m-2 justify-center': true, 'opacity-50 cursor-not-allowed disabled': !permission }">

   
      <img :src="icon" alt="Icon" class="rounded-3xl w-10 h-10 lg:w-20 lg:h-20 border-solid border-2 shadow-xl" />
      
    
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

</style>
