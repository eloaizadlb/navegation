<template>
    <div class="autocomplete">
        <div class="autocomplete-input" @click="toggleList">
            <div v-if="selectedItems.length > 0" class="autocomplete-selected-items">
                <div
                    v-for="(item, index) in selectedItems"
                    :key="item.id"
                    class="autocomplete-selected-item"
                >
                    {{ item.text }}
                    <button class="autocomplete-selected-item-remove" @click="removeSelectedItem(index)">
                        x
                    </button>
                </div>
            </div>
            <input
                type="text"
                :placeholder="placeholderText"
                v-model="searchText"
                @input="searchItems"
                @blur="closeList"
            />
            <div v-if="isLoading" class="autocomplete-loading"></div>
        </div>
        <div v-if="showList" class="autocomplete-list">
            <div
                v-for="item in filteredItems"
                :key="item.id"
                class="autocomplete-list-item"
                @click="selectItem(item)"
            >
                <div class="autocomplete-list-item-text">{{ item.text }}</div>
                <div class="autocomplete-list-item-subtext">{{ item.subtext }}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AutocompleteUI',
    props: {
        items: {
            type: Array,
            required: true,
            default: () => [
                { id: 1, text: 'Apple', subtext: 'Fruit' },
                { id: 2, text: 'Banana', subtext: 'Fruit' },
                { id: 3, text: 'Carrot', subtext: 'Vegetable' },
                { id: 4, text: 'Orange', subtext: 'Fruit' },
                { id: 5, text: 'Broccoli', subtext: 'Vegetable' }
            ]
        },
        placeholderText: {
            type: String,
            default: ''
        },
        isLoading: {
            type: Boolean,
            default: false
        },
        isMultiSelect: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            searchText: '',
            showList: false,
            selectedItems: []
        }
    },
    computed: {
        filteredItems() {
            return this.items.filter(item =>
                item.text.toLowerCase().includes(this.searchText.toLowerCase())
            )
        }
    },
    methods: {
        toggleList() {
            this.showList = !this.showList
        },
        closeList() {
            this.showList = false
        },
        searchItems() {
            this.showList = true
        },
        selectItem(item) {
            if (this.isMultiSelect) {
                if (!this.selectedItems.some(selectedItem => selectedItem.id === item.id)) {
                    this.selectedItems.push(item)
                }
            } else {
                this.selectedItems = [item]
                this.searchText = item.text
                this.showList = false
            }
        },
        removeSelectedItem(index) {
            this.selectedItems.splice(index, 1)
        }
    },
    mounted() {
        document.addEventListener('click', event => {
            if (!this.$el.contains(event.target)) {
                this.showList = false
            }
        })
    }
}
</script>

<style>
.autocomplete {
    position: relative;
    display: inline-block;
}

.autocomplete-input {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px;
    cursor: text;
}

.autocomplete-input input {
    flex: 1;
    border: none;
    outline: none;
    padding: 0 4px;
}

.autocomplete-loading {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    border: 2px solid #ccc;
    border-top-color: #333;
    animation: spin 0.8s linear infinite;
}

.autocomplete-list {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #fff;
    border: 1px solid #ccc;
    border-top: none;
    border-radius: 0 0 4px 4px;
    max-height: 200px;
    overflow-y: auto;
    z-index: 9999; /* Establece una alta z-index para asegurar que esté por encima del contenido */
}


.autocomplete-list-item {
    padding: 4px;
    cursor: pointer;
}

.autocomplete-list-item:hover {
    background-color: #f2f2f2;
}

.autocomplete-list-item-text {
    font-weight: bold;
}

.autocomplete-list-item-subtext {
    font-size: 0.8em;
    color: #666;
}

.autocomplete-selected-items {
    display: flex;
    flex-wrap: wrap;
    margin: 4px 0;
}

.autocomplete-selected-item {
    display: flex;
    align-items: center;
    background-color: #f2f2f2;
    border-radius: 4px;
    padding: 2px 4px;
    margin-right: 4px;
}

.autocomplete-selected-item-remove {
    margin-left: 4px;
    border: none;
    background-color: transparent;
    color: #666;
    cursor: pointer;
}
</style>
