<template>
    <li @click="handleClick" :class="{ clickable: isClickable }">
      <span v-if="!hasIcon">• </span>
      <i v-if="menu.icon" :class="{'fa': true, [menu.icon]: true}"></i>
      {{ menu.nombre }}
      <ul v-if="menu.submenus">
        <menu-item
          v-for="(submenu, index) in menu.submenus"
          :key="index"
          :menu="submenu"
        ></menu-item>
      </ul>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      menu: {
        type: Object,
        required: true
      }
    },
    computed: {
      hasIcon() {
        return !!this.menu.icon;
      },
      isClickable() {
        return !this.menu.submenus || this.menu.submenus.length === 0;
      }
    },
    methods: {
      handleClick() {
        if (this.isClickable) {
          // Realizar acción cuando se hace clic (por ejemplo, navegación)
          console.log('Clicked on:', this.menu.nombre);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Estilos para el menú y submenús */
  /* ... Agrega tus estilos según sea necesario ... */
  </style>
  