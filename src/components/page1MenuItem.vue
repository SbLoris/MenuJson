

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
      link: "/page1",
      permission:"true",
    };
  },
  methods: {
    handleClick() {
      if (this.$el.classList.contains('disabled')) {
        console.log("Element is disabled");
        return;
      } else {
        this.$emit("showToastOnLinkOpen", this.label, this.permission, this.link, this.icon);
        this.$emit("openLink", this.label, this.link);
      }
    },
}
,
};
</script>
