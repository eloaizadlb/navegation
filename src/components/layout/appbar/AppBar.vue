<template>
    <header class="app-bar" :class="['app-bar', { 'expanded': isMenuExpanded }]">
      <ButtonUI text="Crear" type="text"><img src="@/assets/icons/svg/create.svg" alt="Tu imagen SVG" width="20" height="20"/></ButtonUI>
      <ButtonUI text="Aprobar" type="text"><img src="@/assets/icons/svg/aprove.svg" alt="Tu imagen SVG" width="20" height="20"/></ButtonUI>
      <ButtonUI text="Configuracion" type="text"><img src="@/assets/icons/svg/engine.svg" alt="Tu imagen SVG" width="20" height="20"/></ButtonUI>  
      <AvatarUI :avatarUrl="avatarUrl" :list="list"></AvatarUI>
    </header>
  </template>

<script>
import ButtonUI from '../../ui/button/ButtonUI.vue'
import AvatarUI from '../../ui/avatar/AvatarUI.vue'

export default {
  props: {
    isMenuExpanded: {
      type: Boolean,
      default: true // Valor predeterminado: menú expandido
    }
  },
  components: {
    ButtonUI,
    AvatarUI
  },
    data() {
        return {
        avatarUrl: 'https://avatars.githubusercontent.com/u/1024025?v=4',
        list: [
            {
            text: 'Perfil',
            function: () => {
                console.log('Perfil')
            }
            },
            {
            text: 'Cerrar sesión',
            function: () => {
                console.log('Cerrar sesión')
            }
            }
        ]
        }
    }
};
</script>

  <style scoped>
.app-bar {
  width: 100%;
  background-color: #2196F3; /* Color de fondo de la barra superior */
  color: white;
  padding-left: 30px; /* Ajusta el relleno según sea necesario */
  transition: margin-left 0.3s ease, width 0.3s ease; /* Agrega transición a width */
  margin-left: 60px; /* Ancho expandido del menú */
  justify-content: flex-end;
  display: flex;
}

/* Agrega estilos específicos cuando el menú está expandido */
.app-bar.expanded {
  /* padding: 20px; */
  margin-left: 205px; /* Ancho expandido del menú */
  width: calc(100% - 200px); /* Ajusta el ancho del contenido al contraer el menú */
}
  </style> 
  