<template>
  <transition name="modal-fade">
    <div class="modal-mask" v-if="showModal" @click="closeModal($event, false)">
      <div class="modal-wrapper">
        <div class="modal-container" :class="{ 'modal-persistent': persistent }">
          <div class="modal-header">
            <slot name="header"></slot>
            <button class="modal-close" @click="closeModal($event, true)">
              <span>&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <slot></slot>
          </div>
          <div class="modal-footer">
            <slot name="footer"></slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'ModalUI',
  data() {
    return {
      showModal: this.show
    }
  },
  props: {
    show: {
      type: Boolean,
      required: true
    },
    persistent: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    closeModal(event, fromButton) {
      console.log('cerrar')
      if (fromButton) {
        this.showModal = false
        this.$emit('update:show', this.showModal)
      } else {
        const isClickInsideModal = event.target.closest('.modal-container')
        if (!isClickInsideModal) {
          this.showModal = false
          this.$emit('update:show', this.showModal)
        }
      }
    }
  },
  watch: {
    show(newVal) {
      this.showModal = newVal
    }
  }
}
</script>

<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  margin: 10px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  max-width: 50%;
  overflow-y: auto;
  max-height: 50%;
}

.modal-persistent {
  max-height: none;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.modal-close {
  cursor: pointer;
  background: none;
  border: none;
  font-size: 24px;
  font-weight: bold;
  color: #333;
  outline: none;
  padding: 0;
}

.modal-close:hover {
  color: #000;
}

.modal-body {
  margin-bottom: 10px;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
    transition: opacity 0.5s;
}
.modal-fade-enter,
.modal-fade-leave-to {
    opacity: 0;
}
</style>
<!--

    <template>
    <div>
        {{ show }}
        {{ showModal }}
    <transition name="modal-fade">
        <div class="modal" v-if="showModal" @click.self="closeModal">
            <div class="modal-content">
                <slot></slot>
            </div>
        </div>
    </transition>
</div>
</template>

<script>
export default {
    props: {
        show: {
            type: Boolean,
            required: true
        },
        persistent: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            showModal: this.show,
        };
    },
    methods: {
        openModal() {
            this.showModal = true;
        },
        closeModal() {
            this.showModal = false;
      this.$emit('update:show', this.showModal);  // Emitiendo evento para modificar 'show' en el padr
        },
    },
    watch: {
    show(newVal) {
      this.showModal = newVal;
    }
  },
};
</script>

<style>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}
.modal-fade-enter-active,
.modal-fade-leave-active {
    transition: opacity 0.5s;
}
.modal-fade-enter,
.modal-fade-leave-to {
    opacity: 0;
}
</style>

-->