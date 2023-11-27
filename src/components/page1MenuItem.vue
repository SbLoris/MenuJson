<template>
  <icon-with-label-menu-item
    :icon="icon"
    :label="label"
    :permission="permission"
    :link="link"
    @click="handleClick"
  />
</template>

<script>
import IconWithLabelMenuItem from "@/components/IconWithLabelMenuItem.vue";

export default {
  components: {
    IconWithLabelMenuItem,
  },
  data() {
    return {
      icon: require("@/assets/fruit.jpg"),
      label: "Page 1",
      permission: true,
      link: "/config",
    };
  },
  methods: {
  handleClick() {
    if (this.permission) {
      this.$emit("showToastOnLinkOpen", this.label, this.permission);
    }
  },
  
},

};
</script>
