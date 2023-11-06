<!-- <script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script> -->

<template>
  <div id="app">
    <app-bar :is-menu-expanded="isMenuExpanded"/>
    <menu-layout  @menu-toggle="handleMenuToggle"/>
    <content-section :is-menu-expanded="isMenuExpanded"/>
    <footer-layout />
  </div>
</template>

<script>
import AppBar from './components/layout/appbar/AppBar.vue';
import MenuLayout from './components/layout/menu-layout/MenuLayout.vue';
import ContentSection from './components/layout/content/ContentSection.vue';
import FooterLayout from './components/layout/footer-layout/FooterLayout.vue';

export default {
  components: {
    AppBar,
    MenuLayout,
    ContentSection,
    FooterLayout
  },
  data() {
    return {
      isMenuExpanded: true
    };
  },
  methods: {
    handleMenuToggle(isExpanded) {
      this.isMenuExpanded = isExpanded;
      console.log('Menú expandido:', isExpanded);
    }
  }
};
</script>

<style>
/* Estilos globales si es necesario */
body {
  margin: 0;
  font-family: Arial, sans-serif;
}
</style>
