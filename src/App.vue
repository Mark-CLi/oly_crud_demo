<template>
  <div class="container">
    <h1 class="title">Olympus Dev Application CRUD Demo</h1>
    <h2 class="text">By entering a phone numbers, a request to AbstractAPI will be sent and the phone you submitted will be transmitted. AbstractAPI ToS may apply</h2>
    <ItemForm @addItem="addItem" />
    <ItemList
        :items="items"
        @deleteItem="deleteItem"
        @editItem="startEdit"
    />
    <ItemEdit
        v-if="editingItem"
        :item="editingItem"
        @updateItem="updateItem"
        @cancelEdit="cancelEdit"
    />
  </div>
</template>

<script>
import { reactive, ref } from 'vue';
import ItemForm from './components/ItemForm.vue';
import ItemList from './components/ItemList.vue';
import ItemEdit from './components/ItemEdit.vue';

export default {
  components: { ItemForm, ItemList, ItemEdit },
  setup() {
    const items = reactive([]); // make array to store items
    const editingItem = ref(null); // ref to store the item being edited

    const addItem = (item) => { // add item to the items array
      items.push({ id: Date.now(), ...item });
    };

    const deleteItem = (id) => {
      const index = items.findIndex((item) => item.id === id);
      items.splice(index, 1);
    };

    const startEdit = (item) => { // start editing the item
      editingItem.value = { ...item };
    };

    const updateItem = (updatedItem) => { // update the item
      const index = items.findIndex((item) => item.id === updatedItem.id); // find the index of the item
      items[index] = updatedItem;
      editingItem.value = null;
    };

    const cancelEdit = () => { // cancel the edit
      editingItem.value = null;
    };

    return {items, editingItem, addItem, deleteItem, startEdit, updateItem, cancelEdit};
  },
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.title {
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: center;
}

.text {
  text-align: center;
  font-size: 1.2em;
  margin-bottom: 20px;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>