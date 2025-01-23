<template>
  <ul class="item-list">
    <li v-for="item in items" :key="item.id" class="item">
      <div v-if="editingItem && editingItem.id === item.id">
        <input v-model="editingItem.title" class="input-field" />
        <p><strong>Country Code:</strong> {{ item.code }}</p>
        <p><strong>Country Name:</strong> {{ item.name }}</p>
        <p><strong>Prefix:</strong> {{ item.prefix }}</p>
        <p><strong>Location:</strong> {{ item.location }}</p>
        <p><strong>Carrier:</strong> {{ item.carrier }}</p>
        <textarea v-model="editingItem.note" placeholder="Enter Note" class="note-field"></textarea>
        <button @click="saveEdit" class="save-button">Save</button>
        <button @click="cancelEdit" class="cancel-button">Cancel</button>
      </div>
      <div v-else>
        <p><strong>Phone Number:</strong> {{ item.title }}</p>
        <p><strong>Country Code:</strong> {{ item.code }}</p>
        <p><strong>Country Name:</strong> {{ item.name }}</p>
        <p><strong>Prefix:</strong> {{ item.prefix }}</p>
        <p><strong>Location:</strong> {{ item.location }}</p>
        <p><strong>Carrier:</strong> {{ item.carrier }}</p>
        <p><strong>Note:</strong> {{ item.note }}</p>
        <button @click="startEdit(item)" class="edit-button">Edit</button>
        <button @click="showConfirmDialog(item.id)" class="delete-button">Delete</button>
      </div>
    </li>
    <ConfirmDialog v-if="confirmDialogVisible" :message="'Are you sure you want to delete this item?'" @confirm="deleteItem" @cancel="hideConfirmDialog" />
  </ul>
</template>

<script>
import { ref } from 'vue'; // import ref function from vue Composition API????
import ConfirmDialog from './ConfirmDialog.vue'; // import the ConfirmDialog component

export default {
  components: { ConfirmDialog },
  props: ['items'],
  emits: ['updateItem', 'deleteItem'],
  setup(props, { emit }) {
    // set variables
    const editingItem = ref(null);
    const confirmDialogVisible = ref(false);
    const itemIdToDelete = ref(null);

    // start editing on user click
    const startEdit = (item) => {
      editingItem.value = { ...item };
    };

    // save the edit and update the item via api
    const saveEdit = async () => {
      try {
        const response = await fetch(`https://phonevalidation.abstractapi.com/v1/?api_key=6de0f1aeb85d421eac2f9831806394c1&phone=${editingItem.value.title}`);
        const data = await response.json();
        if (data.valid) {
          editingItem.value.code = data.country.code;
          editingItem.value.name = data.country.name;
          editingItem.value.prefix = data.country.prefix;
          editingItem.value.location = data.location;
          editingItem.value.carrier = data.carrier;
        } else {
          console.error('Invalid phone number');
        }
      } catch (err) {
        console.error('API request failed', err);
      }

      emit('updateItem', editingItem.value);
      const index = props.items.findIndex(item => item.id === editingItem.value.id);
      if (index !== -1) {
        props.items[index] = { ...editingItem.value };
      }
      editingItem.value = null;
    };
    // cancel the edit
    const cancelEdit = () => {
      editingItem.value = null;
    };

    // show confirm dialog
    const showConfirmDialog = (id) => {
      itemIdToDelete.value = id;
      confirmDialogVisible.value = true;
    };

    // hide confirm dialog
    const hideConfirmDialog = () => {
      confirmDialogVisible.value = false;
      itemIdToDelete.value = null;
    };

    // delete the item
    const deleteItem = () => {
      emit('deleteItem', itemIdToDelete.value);
      hideConfirmDialog();
    };

    return { editingItem, startEdit, saveEdit, cancelEdit, showConfirmDialog, hideConfirmDialog, deleteItem, confirmDialogVisible };
  },
};
</script>

<style scoped>
.item-list {
  list-style-type: none;
  padding: 0;
}

.item {
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}

.item p {
  margin: 5px 0;
}

.input-field, .note-field {
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 100%;
  max-width: 300px;
}

.edit-button, .delete-button, .save-button, .cancel-button {
  margin-right: 5px;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.edit-button {
  background-color: #4CAF50;
  color: white;
}

.delete-button {
  background-color: #f44336;
  color: white;
}

.save-button {
  background-color: #2196F3;
  color: white;
}

.cancel-button {
  background-color: #9E9E9E;
  color: white;
}
</style>