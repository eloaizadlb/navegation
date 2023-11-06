<template>
  <aside class="menu" :class="{ expanded: isMenuExpanded }">
    <i
      class="mdi"
      :class="{ 'mdi-menu': !isMenuExpanded, 'mdi-backburger': isMenuExpanded }"
      @click="toggleMenu"
      style="font-size: 2rem"
    ></i>
    show: {{ showLogoKangu }}
    <menu-recursivo :menus="menus" :isMenuExpanded="isMenuExpanded"></menu-recursivo>
    <transition name="fade">
    <img v-if="showLogoKangu" :src="imageSrc" alt="Tu imagen SVG" class="menu-footer-image" @load="imageLoaded"/>
  </transition>
  </aside>
</template>

<script>
import MenuRecursivo from '../menu-recursivo/MenuRecursivo.vue'
import menu from '@/menu'

export default {
  created() {
    import('@/assets/icons/svg/logo_blanco.svg')
      .then((src) => {
        this.imageSrc = src.default
      })
      .catch((error) => {
        console.error('Error al cargar la imagen:', error)
      })
  },
  data() {
    return {
      menus: menu.menu,
      isMenuExpanded: true,
      imageSrc: '',
      showLogoKangu: true
    }
  },
  components: {
    MenuRecursivo
  },
  methods: {
    toggleMenu() {
      this.isMenuExpanded = !this.isMenuExpanded
      this.$emit('menu-toggle', this.isMenuExpanded) // Emitir el evento con el estado actual del menú
      this.getSvg()
    },
    getSvg() {
      console.log('getSvg')
      if (this.isMenuExpanded) {
        import(`@/assets/icons/svg/logo_blanco.svg`)
          .then((src) => {
            this.imageSrc = src.default
          })
          .catch((error) => {
            console.error('Error al cargar la imagen:', error)
          })
      } else {
        import(`@/assets/icons/svg/isotipo_kangusoft_blanco.svg`)
          .then((src) => {
            this.imageSrc = src.default
          })
          .catch((error) => {
            console.error('Error al cargar la imagen:', error)
          })
      }
      this.showLogoKangu = true
    }
  }
}
</script>

<style scoped>
.menu {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%; /* Establece la altura al 100% */
  width: 60px; /* Ancho inicial del menú */
  background-color: #333; /* Color de fondo del menú */
  color: white;
  padding: 5px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  z-index: 1; /* Establece un z-index superior para que esté delante del pie de página */
  transition: width 0.3s ease; /* Agrega una transición suave a la propiedad width */
}
.menu.expanded {
  width: 200px; /* Ancho expandido del menú */
}
.menu-toggle {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 24px;
  padding: 5px;
  width: 100%;
  text-align: left;
  outline: none;
}
.menu-toggle i {
  margin-left: 10px;
}
.menu ul {
  list-style: none;
  padding: 0;
  flex: 1; /* Hace que la lista ocupe el espacio restante verticalmente */
  display: flex;
  flex-direction: column;
  width: 100%; /* Ajusta el ancho de la lista */
}
.menu ul li {
  padding: 10px;
}
.menu ul li:hover {
  background-color: #555;
}
.menu ul li a {
  color: white;
  text-decoration: none;
}
.menu ul li a:hover {
  color: #2196f3;
}
.menu.expanded {
  width: 200px; /* Ancho expandido del menú */
}
.menu-toggle .toggle-text {
  margin-left: 10px;
}
.menu-footer-image {
  /* margin-top: auto; Para que la imagen se alinee en la parte inferior del menú */
  margin-bottom: 20%;
}
</style>
