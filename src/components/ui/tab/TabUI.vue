<template>
    <div class="tab-ui">
        {{ activeTab }}
        <div class="tab-header">
            <div
                v-for="(item, index) in items"
                :key="index"
                :class="['tab-item', { active: activeTab === index }, { disabled: item.disable }]"
                @click="handleTabClick(item)"
            >
                <div class="tab-icon">{{ item.icon }}</div>
                <div class="tab-title">{{ item.title }}</div>
                <div class="tab-subtitle">{{ item.subtitle }}</div>
            </div>
        </div>
        <div class="tab-content">
            <slot :activeTab="activeTab"></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TabUI',
    props: {
        items: {
            type: Array,
            required: true,
            default: () => [
                // { id: 1, title: 'Tab 1', subtitle: 'Subtitle 1', icon: 'Icon 1', disable: false },
                // { id: 2, title: 'Tab 2', subtitle: 'Subtitle 2', icon: 'Icon 2', disable: false },
                // { id: 3, title: 'Tab 3', subtitle: 'Subtitle 3', icon: 'Icon 3', disable: false },
            ],
        },
    },
    data() {
        return {
            activeTab: 0,
        };
    },
    methods: {
        handleTabClick(item) {
            console.log('item: ', item.id);
            console.log('item:', JSON.parse(JSON.stringify(item)));
                    if (!item.disable && item.onClick) {
        item.onClick(item); // Ejecutar la función onClick con el ítem como argumento
        this.activeTab = this.items.findIndex((tab) => tab === item);
      }
    },
    },
};
</script>

<style scoped>
.tab-ui {
    display: flex;
    flex-direction: column;
    height: 100%;
}

.tab-header {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    padding: 10px;
    background-color: #f5f5f5;
}

.tab-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 10px;
    cursor: pointer;
    border-radius: 5px;
    transition: all 0.2s ease-in-out;
}

.tab-item:hover {
    background-color: #e0e0e0;
}

.tab-item.active {
    background-color: #fff;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.tab-item.disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.tab-icon {
    font-size: 24px;
    margin-bottom: 5px;
}

.tab-title {
    font-size: 14px;
    font-weight: bold;
}

.tab-subtitle {
    font-size: 12px;
    color: #666;
}

/* .tab-content {
    flex: 1;
    padding: 10px;
    background-color: #fff;
    overflow-y: auto;
} */
.tab-content {
  /* Borders */
  border: 1px solid #ccc;
  border-radius: 5px;

  /* Shadow */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);

  /* Background color */
  background-color: #fff;

  /* Padding */
  padding: 10px;

  /* Overflow */
  overflow-y: auto;
    /* Añadir transición */
    transition: opacity 0.3s ease;
}
.tab-content.fade-enter-active,
.tab-content.fade-leave-active {
  transition: opacity 0.3s ease;
}

.tab-content.fade-enter, .tab-content.fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
</style>

