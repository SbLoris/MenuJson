// page5MenuItem.vue

<template>
  <icon-with-label-menu-item
    :icon="icon"
    :label="label"
    
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
      link: "/config",
    };
  },
  methods: {
  handleClick() {
    console.log("handleClick method called");
    
    // Vérifiez si l'élément fait partie de la classe 'disabled'
    if (this.$el.classList.contains('disabled')) {
      console.log("Element is disabled");
      return;
    } else {
      // Si l'élément n'est pas désactivé, émettez l'événement pour déclencher le toast
      this.$emit("showToastOnLinkOpen", this.label);
    }
  }
}

};
</script>
