<template>
    <main :class="{'content-section': true, 'content-section-expanded': isMenuExpanded}">
      <!-- Contenido de la sección principal -->
      <!-- <BreadcrumbUI></BreadcrumbUI>
      <SkeletonUI type="card"></SkeletonUI>
      {{ showModal }}
      <button @click="changeShowModal()">Mostrar modal</button>
      <AutocompleteUI></AutocompleteUI> -->
      <TabUI :items="tabItems">
      <!-- Contenido para cada pestaña -->
      <template v-slot:default="{ activeTab }">
      <div v-if="activeTab === 0">
        Este es el contenido del tab 1.
      </div>
      <div v-else-if="activeTab === 1">
        Este es el contenido del tab 2.
      </div>
      <div v-else-if="activeTab === 2">
        Este es el contenido del tab 3.
      </div>
    </template>
    </TabUI>
     vmodel:  <strong>{{ localVal }}</strong> xxx
      <InputUI v-model="localVal" type="number"></InputUI>
      <DatePieckerUI type="range" :allow-before="true"></DatePieckerUI>
      <ModalUI v-model:show="showModal" :persistent="false">
        <h2>Hola Modal</h2>
      </ModalUI> 
      <router-view></router-view>
    </main>
  </template>

<script>
import BreadcrumbUI from '../../ui/breadcrumb/BreadcrumbUI.vue'
import SkeletonUI from '../../ui/skeleton/SkeletonUI.vue'
import ModalUI from '../../ui/modal/ModalUI.vue'
import DatePieckerUI from '../../ui/date-picker/DatePickerUI_3.vue'
import AutocompleteUI from '../../ui/autocomplete/AutocompleteUI.vue'
import InputUI from '../../ui/input/InputUI_2.vue'
import TabUI from '../../ui/tab/TabUI.vue'

export default {
  props: {
    isMenuExpanded: {
      type: Boolean,
      default: true // Valor predeterminado: menú expandido
    }
  },
  data(){
    return {
      showModal: false,
      vModelAutocomplete: null,
      localVal: '',
      tabItems: [
        { id: 1, title: 'Tab 1', subtitle: 'Subtitle 1', icon: 'Icon 1', disable: false, onClick: this.handleClick },
        { id: 2, title: 'Tab 2', subtitle: 'Subtitle 2', icon: 'Icon 2', disable: false, onClick: this.handleClick },
        { id: 3, title: 'Tab 3', subtitle: 'Subtitle 3', icon: 'Icon 3', disable: true, onClick: this.handleClick },
      ],
    } 
  },
  components: {
    TabUI,
    InputUI,
    AutocompleteUI,
    BreadcrumbUI,
    SkeletonUI,
    ModalUI,
    DatePieckerUI
  },
  methods: {
    handleClick(item) {
      console.log('Tab clicked:', item);
      // Tu lógica para manejar el clic en la pestaña
    },
  }
};
</script>
  
<style scoped>
.content-section {
  width: 100%;
  padding-left: 30px;
  z-index: 6;
  transition: margin-left 0.3s ease, width 0.3s ease; /* Agrega transición a width */
}

.content-section-expanded {
  margin-left: 205px; /* Ancho expandido del menú */
  width: calc(100% - 200px); /* Ajusta el ancho del contenido al contraer el menú */
}

.content-section:not(.content-section-expanded) {
  margin-left: 60px; /* Ancho del menú */
  width: calc(100% - 60px); /* Ajusta el ancho del contenido al expandir el menú */
}
  </style>
  