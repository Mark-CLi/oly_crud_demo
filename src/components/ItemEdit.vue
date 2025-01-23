<template>
  <form @submit.prevent="submitEdit">
    <input v-model="localItem.title" class="input-field" />
    <p><strong>Country Code:</strong> {{ localItem.code }}</p>
    <p><strong>Country Name:</strong> {{ localItem.name }}</p>
    <p><strong>Prefix:</strong> {{ localItem.prefix }}</p>
    <p><strong>Location:</strong> {{ localItem.location }}</p>
    <p><strong>Carrier:</strong> {{ localItem.carrier }}</p>
    <textarea v-model="localItem.note" placeholder="Enter Note" class="note-field"></textarea>
    <button type="submit" class="save-button">Save</button>
    <button type="button" @click="$emit('cancelEdit')" class="cancel-button">Cancel</button>
  </form>
</template>

<script>
import { reactive, toRefs, watch } from 'vue';

export default {
  props: ['item'], // get props from parent
  emits: ['updateItem', 'cancelEdit'], // set event to emit to parent
  setup(props, { emit }) {
    const localItem = reactive({ ...props.item }); // make a copy of the item prop so we are not editing the whole thing

    watch(localItem, (newVal) => { // watch for changes in the localItem(loclastorage?
      emit('updateItem', { ...newVal });  // emit updateItem event
    }, { deep: true }); //watch all changes

    const submitEdit = () => {
      emit('updateItem', { ...localItem });
    };

    return { ...toRefs(localItem), submitEdit }; // return
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

.input-field, .note-field {
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 100%;
  max-width: 300px;
}

.save-button, .cancel-button {
  margin-right: 5px;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
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