<template>
  <div id="app" class="h-screen bg-sky-100">
    <div id="toast-container">
    
    </div> 
    <div class="menu-whole h-5/6 ">
      <div class="field-loader w-1/2 h-full shadow-xl ">
        <textarea  v-model="jsonInput" placeholder="Entrez le JSON du menu ici" class="block p-2.5 h-full w-full "></textarea>
        <button class="bg-blue-500 hover:bg-blue-400 text-white font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded"  @click="loadMenu">Charger le menu</button>
      </div>
      <div class="menu-loaded w-1/2 text-gray-900 bg-blue-500 rounded shadow-xl p-8" >
        <div v-for="(item, index) in getMenuItems()" :key="index">
          <component
            :is="getComponentName(item)"
            :label="item.label"
            :permission="item.permission"
            :link="item.link"
            :articleCount="item.component === 'nbArticlesLabel' ? dynamicArticleCount : undefined"
            @updateArticleCount="updateArticleCount"
            @showToastOnLinkOpen="showToastOnLinkOpen"
          />
        </div>
        <div class="flex justify-center">
        <button class="bg-white " @click="toggleHiddenItems" v-if="hiddenItems.length > 0">Other</button>
        </div>
        <component :is="dynamicComponent" v-if="dynamicComponent" />
      </div>
    </div>
    
  </div>
</template>

<script>
import nbArticlesLabel from "@/components/nbArticlesLabel.vue";
import nbArticlesField from "@/components/nbArticlesFields.vue";
import homeMenuItem from "@/components/homeMenuItem.vue";
import page1MenuItem from "@/components/page1MenuItem.vue";
import page4MenuItem from "@/components/page4MenuItem.vue";
import page5MenuItem from "@/components/page5MenuItem.vue";

export default {
  components: {
    nbArticlesLabel,
    nbArticlesField,
    homeMenuItem,
    page1MenuItem,
    page4MenuItem,
    page5MenuItem,
  },
  data() {
    return {
      dynamicArticleCount: 0,
      jsonInput: "",
      menuItems: [],
      hiddenItems: [],
      showHiddenItems: false,
      dynamicComponent: null,
    };
  },
  methods: {
    loadMenu() {
      try {
        const parsedJson = JSON.parse(this.jsonInput);
        this.menuItems = parsedJson.menuItems.map((item) => ({ ...item, type: "menuItem" }));
        this.hiddenItems = parsedJson.hiddenItems.map((item) => ({ ...item, type: "hiddenItem" }));
        this.showHiddenItems = false;
        

      } catch (error) {
        console.error("Erreur lors du chargement du JSON :", error);
      }
    },
    toggleHiddenItems() {
      this.showHiddenItems = !this.showHiddenItems;
    },
    getMenuItems() {
      const combinedItems = [...this.menuItems, ...this.hiddenItems];
      return this.showHiddenItems ? combinedItems : this.menuItems;
    },
    updateArticleCount(value) {
      this.dynamicArticleCount = value;
    },
    showToastOnLinkOpen(message) {
      this.showToast(message);
    },
    showToast(message) {
 
    const toastElement = document.createElement("div");
    toastElement.className = "toast fixed flex items-center w-full max-w-xs p-4 space-x-4 text-gray-500 bg-white divide-x rtl:divide-x-reverse divide-gray-200 rounded-lg shadow top-5 right-5 dark:text-gray-400 dark:divide-gray-700 space-x dark:bg-gray-800";
    toastElement.textContent = `Ouverture de la page ${message}`;
    
   
    this.$emit("showToastOnLinkOpen", `Ouverture de la page ${message}`);

    document.getElementById("toast-container").appendChild(toastElement);

    setTimeout(() => {
      toastElement.remove();
    }, 5000);
  
},
    getComponentName(item) {
      // Mapping entre les composants et leurs noms
      const componentMap = {
        homeMenuItem,
        page1MenuItem,
        nbArticlesField,
        page4MenuItem,
        page5MenuItem,
        nbArticlesLabel,
      };
      return componentMap[item.component] || null;
    },
  },
};
</script>
<style lang="css">
  @import '@/assets/styles/tailwind.css';
  
</style>


