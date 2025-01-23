<template>
  <div v-if="visible" class="modal-overlay">
    <div class="modal">
      <p>{{ message }}</p>
      <button @click="confirm" class="confirm-button">Yes</button>
      <button @click="cancel" class="cancel-button">No</button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  props: ['message'], // show dialolg
  emits: ['confirm', 'cancel'], // emit confirm and cancel events and tell other componets
  setup(props, { emit }) {
    const visible = ref(true); // state of the dialog

    const confirm = () => { // confirm the dialog
      emit('confirm'); // emit confirm event
      visible.value = false; // hide the dialog
    };

    const cancel = () => { // cancel the dialog
      emit('cancel'); // emit cancel event
      visible.value = false; // hide the dialog
    };

    return { visible, confirm, cancel };
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}

.confirm-button {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px;
}

.cancel-button {
  background-color: #f44336;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style>