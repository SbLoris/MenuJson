<template>
  <icon-with-label-menu-item
    :icon="icon"
    :label="label"
    :link="link"
    :permission="permission"
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
      icon: require("@/assets/maison.jpg"),
      label: "Accueil",
      link: "/home",
      permission:"true",
    };
  },
  methods: {
 
    handleClick() {
      if (this.$el.classList.contains('disabled')) {
        console.log("Element is disabled");
        return;
      } else {
        // If the element is not disabled, emit the event to trigger the toast
        this.$emit("showToastOnLinkOpen", this.label, this.permission, this.link, this.icon);
        this.$emit("openLink", this.label, this.link);
      }
    },
},

};
</script>
