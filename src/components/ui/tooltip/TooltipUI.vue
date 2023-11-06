<template>
    <div class="tooltip-container" @mouseenter="show" @mouseleave="hide">
      <div class="tooltip-content">
        <slot></slot>
      </div>
      <div
        v-if="showTooltip"
        :class="['tooltip', tooltipPositionClass]"
        :style="{ backgroundColor: bgColor, width: width + 'px', height: height + 'px' }"
      >
        {{ text }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TooltipUI',
    props: {
      text: {
        type: String,
        required: true
      },
      bgColor: {
        type: String,
        default: '#333'
      },
      width: {
        type: Number,
        default: 100
      },
      height: {
        type: Number,
        default: 50
      },
      position: {
        type: String,
        default: 'top' // Valor predeterminado: se mostrará arriba
      }
    },
    data() {
      return {
        showTooltip: false
      }
    },
    computed: {
      tooltipPositionClass() {
        return `tooltip-${this.position}`;
      }
    },
    methods: {
      show() {
        this.showTooltip = true;
      },
      hide() {
        this.showTooltip = false;
      }
    }
  };
  </script>
  
  <style scoped>
  .tooltip-container {
    position: relative;
    display: inline-block; /* Mantén el tooltip dentro del flujo inline */
  }
  
  .tooltip-content {
    display: inline-block; /* Mantén el contenido dentro del flujo inline */
  }
  
  .tooltip {
    position: absolute;
    color: #fff;
    padding: 10px;
    border-radius: 5px;
    text-align: center;
    font-size: 10px;
    z-index: 999;
  }
  
  /* Estilos de posición del tooltip */
  .tooltip-top {
    top: -200%;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .tooltip-bottom {
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .tooltip-left {
    top: 50%;
    right: 100%;
    transform: translateY(-50%);
  }
  
  .tooltip-right {
    top: 50%;
    left: 100%;
    transform: translateY(-50%);
  }
  </style>
  