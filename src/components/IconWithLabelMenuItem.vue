<template>
  <div @click="handleClick" :class="{ 'disabled': !permission }">
    <img :src="icon" alt="Icon" />
    <span>{{ label }}</span>
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
    handleClick() {
      if (this.permission) {
        this.$emit("openLink", this.label, this.link);
      }
    },
  },
  computed: {
    iconClass() {
      return this.$options.name && this.$options.name.includes('MenuItem') ? 'home-icon' : 'default-icon';
    },
  },
};
</script>

<style scoped>
.home-icon {
  background-color: red;
}

.default-icon {
  background-color: blue;
}

.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
