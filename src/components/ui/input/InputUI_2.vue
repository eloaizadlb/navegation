<template>
  <div>
    <div v-if="type == 'number'"  class="input-prepend">
        <span v-if="symbol.length > 0" class="prepend-symbol" :style="{ backgroundColor: symbolBackgroundColor }">{{ symbol }}</span>
      <input
        type="text"
        v-model="formati"
        @input="updateValueNumber"
        @update:model-value="updateValue"
        @keydown="handleKeyDown"
      />
    </div>
    <div v-else-if="type == 'text'">
      <input type="text" v-model="texto" @update:model-value="updateValueText" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    },
    symbol: {
      type: String,
      default: '$'
    },
    symbolBackgroundColor: {
      type: String,
      default: '#f0f0f0' // Color de fondo predeterminado (tenue)
    }
  },
  data() {
    return {
      formattedValue: this.formatValue(this.value),
      otroValor: '',
      formati: '',
      texto: '',
      cal: 'ssss'
    }
  },
  methods: {
    handleKeyDown(event) {
      const keyCode = event.keyCode

      // Permitir números (códigos ASCII del 0 al 9), coma (código ASCII 188),
      // backspace (código ASCII 8) y borrar (código ASCII 46)
      if (
        (keyCode >= 48 && keyCode <= 57) ||
        keyCode === 188 ||
        keyCode === 8 || // backspace
        keyCode === 46 // borrar
      ) {
        return
      }

      // Prevenir la acción predeterminada para otros caracteres
      event.preventDefault()
    },
    formatValue(value) {
      return value.toLocaleString()
    },
    updateValueNumber(event) {
      this.otroValor = event.target.value.replace(/\./g, '')
      console.log('this.otroValor: ', this.otroValor)
      const parts = this.otroValor.split(',')
      let integerPart = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, '.')

      let decimalPart = ''
      if (parts.length > 1) {
        // Limitar la parte decimal a dos dígitos
        decimalPart = `,${parts[1].slice(0, 2)}`
      }
      console.log('decimalPart: ', decimalPart)
      console.log('integerPart: ', integerPart)

      this.formati = decimalPart ? `${integerPart}${decimalPart}` : integerPart
      // this.formati = value.replace(/\B(?=(\d{3})+(?!\d))/g, '.');
      console.log('formati: ', this.formati)

      this.formattedValue = this.formatValue(this.otroValor)
      this.$emit('update:model-value', event.target.value.replace(/\./g, ''))
    },
    updateValueText() {
      this.$emit('update:model-value', this.texto)
    }
  }
}
</script>
<style scoped>
.input-prepend {
  display: flex;
  align-items: center;
  /* border: 1px solid #ccc; */
  border-radius: 5px;
  padding: 5px;
}

.prepend-symbol {
  margin-right: 1px; /* Ajusta el espacio entre el símbolo y el input */
  padding: 2px; /* Añade un poco de espacio alrededor del símbolo */
  background-color: #f0f0f0; /* Color de fondo tenue */
  border: 1px solid #ccc; /* Borde */
}
</style>