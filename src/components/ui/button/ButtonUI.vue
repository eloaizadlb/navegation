<template>
  <button :class="buttonClass" :style="buttonStyle">
    <span class="text-button">
      {{ buttonText }}
      <slot> </slot>
    </span>
  </button>
</template>

<script>
export default {
  name: 'ButtonUI',
  props: {
    text: {
      type: String,
      required: true
    },
    type: {
      type: String,
      default: 'filled',
      validator: (value) => {
        return ['filled', 'outlined','text'].includes(value)
      }
    },
    width: {
      type: Number,
      default: 120
    },
    height: {
      type: Number,
      default: 30
    },
    fontsize: {
      type: Number,
      default: 10
    }
  },
  computed: {
    buttonClass() {
      return `button-${this.type}`
    },
    buttonText() {
      return this.text.toUpperCase()
    },
    buttonStyle() {
      return {
        width: `${this.width}px`,
        height: `${this.height}px`,
        'font-size': `${this.fontsize}px`
      }
    }
  }
}
</script>

<style scoped>
.button-filled {
  background-color: #007aff;
  color: #fff;
  border: none;
  border-radius: 4px;
  /* padding: 12px 24px; */
  cursor: pointer;
}

.button-outlined {
  background-color: transparent;
  color: #007aff;
  border: 2px solid #007aff;
  border-radius: 4px;
  /* padding: 12px 24px; */
  cursor: pointer;
}

.button-text {
  background-color: transparent;
  color: #007aff;
  border: none;
  /* padding: 12px 24px; */
  cursor: pointer;
}
.button-text:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.text-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}
</style>
