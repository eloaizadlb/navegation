<template>
  <ul>
    <li v-for="(menu, index) in menus" :key="index" type="none">
      <i :class="getStateToggle(menu)" v-if="hasSubmenus(menu)" @click="toggleSubMenu(menu)"></i>
      <TooltipUI :text="menu.nombre" bgColor="#FFA500" :width="50" :height="20">
        <i class="mdi mdi-home"></i>
      </TooltipUI>
      <router-link
        class="no-underline"
        :class="{ 'selected': isMenuSelected(menu.path) }"
        v-if="menu.path && this.isMenuExpanded"
        :to="menu.path"
        @click="menuClickHandler(menu)"
      >
        <span v-if=" isMenuSelected(menu.path)">🌟</span> <span> {{ menu.nombre }}</span>
      </router-link>
      <span v-if="!menu.path && this.isMenuExpanded"> {{ menu.nombre }}</span>
      <menu-recursivo
        v-if="hasSubmenus(menu) && menu.expanded"
        :menus="menu.submenus"
        :isMenuExpanded="isMenuExpanded"
        class="no-underline"
      ></menu-recursivo>
    </li>
  </ul>
</template>

<script>
import TooltipUI from '../../ui/tooltip/TooltipUI.vue'

export default {
  props: {
    menus: {
      type: Array,
      required: true
    },
    isMenuExpanded: {
      type: Boolean,
      required: true
    }
  },
  components: {
    TooltipUI
  },
  methods: {
    hasIcon(menu) {
      return !!menu.icon
    },
    hasSubmenus(menu) {
      return menu.submenus && menu.submenus.length > 0
    },
    toggleSubMenu(menu) {
      if (this.hasSubmenus(menu)) {
        menu.expanded = !menu.expanded
      }
    },
    menuClickHandler(menu) {
      // Puedes realizar acciones adicionales al hacer clic en un menú sin submenús
      console.log('Clic en menú:', menu)
    },
    isMenuSelected(menuPath) {
    return this.$route.path === menuPath;
  },
    getStateToggle(menu) {
      return menu.expanded ? 'mdi mdi-chevron-right' : 'mdi mdi-chevron-down'
    },
    getMenuItemStyle(menuPath) {
    return this.isMenuSelected(menuPath) ? { backgroundColor: 'white' } : {};
  }
  }
}
</script>

<style scoped>
.icon-collapsed {
  transform: rotate(-90deg);
}
span,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}
.no-underline {
  text-decoration: none;
}
.selected {
  background-color: white; /* Fondo blanco para el texto seleccionado */
}
/* Estilos para el menú y submenús */
/* ... Agrega tus estilos según sea necesario ... */
</style>
