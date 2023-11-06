<template>
    <div class="autocomplete-container">
      <div class="autocomplete-input">
        <input
          v-model="searchTerm"
          @input="filterItems"
          :placeholder="placeholder"
          :disabled="loading"
        />
      </div>
      <div v-if="multiple" class="selected-items">
        <div
          v-for="selectedItem in selectedItems"
          :key="selectedItem.id"
          class="chip"
        >
          {{ selectedItem.text }}
          <button @click="removeSelectedItem(selectedItem)" class="close-btn">
            x
          </button>
        </div>
      </div>
      <ul v-if="filteredItems.length > 0" class="autocomplete-list">
        <li
          v-for="item in filteredItems"
          :key="item.id"
          @click="selectItem(item)"
        >
          {{ item.text }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      items: {
        type: Array,
        required: true,
        default: () => [
  { id: 1, text: 'Apple', subtext: 'Fruit', tooltipText: 'A fruit from an apple tree' },
  { id: 2, text: 'Banana', subtext: 'Fruit', tooltipText: 'A fruit from a banana plant' },
  { id: 3, text: 'Carrot', subtext: 'Vegetable', tooltipText: 'A root vegetable, usually orange in color' },
  { id: 4, text: 'Orange', subtext: 'Fruit', tooltipText: 'A citrus fruit known for its orange color and sweet taste' },
  { id: 5, text: 'Broccoli', subtext: 'Vegetable', tooltipText: 'A green vegetable with a tree-like shape' },
  // Add more items as needed
]
      },
      placeholder: {
        type: String,
        default: "Enter text"
      },
      multiple: {
        type: Boolean,
        default: false
      },
      loading: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        searchTerm: "",
        filteredItems: [],
        selectedItems: []
      };
    },
    methods: {
      filterItems() {
        this.filteredItems = this.items.filter(item =>
          item.text.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      },
      selectItem(item) {
        if (this.multiple) {
          this.selectedItems.push(item);
          this.filteredItems = this.filteredItems.filter(
            filteredItem => filteredItem.id !== item.id
          );
        }
      },
      removeSelectedItem(selectedItem) {
        this.selectedItems = this.selectedItems.filter(
          item => item.id !== selectedItem.id
        );
        this.filteredItems.push(selectedItem);
      }
    }
  };
  </script>
  
  <style scoped>
  .autocomplete-container {
    position: relative;
    display: inline-block;
    width: 300px;
  }
  
  .autocomplete-input input {
    width: 100%;
    padding: 10px;
  }
  
  .autocomplete-list {
    list-style-type: none;
    padding: 0;
  }
  
  .autocomplete-list li {
    padding: 10px;
    cursor: pointer;
  }
  
  .autocomplete-list li:hover {
    background-color: #f0f0f0;
  }
  
  .selected-items {
    display: flex;
    flex-wrap: wrap;
    margin-top: 10px;
  }
  
  .chip {
    background-color: #e0e0e0;
    color: #333;
    border-radius: 16px;
    padding: 5px 10px;
    margin: 5px;
    display: flex;
    align-items: center;
  }
  
  .close-btn {
    background: none;
    border: none;
    color: red;
    padding: 0;
    margin-left: 5px;
    cursor: pointer;
  }
  </style>
  