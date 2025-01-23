<template>
  <form @submit.prevent="submitForm" class="item-form">
    <input v-model="title" placeholder="Enter Phone Number" class="input-field" />
    <button type="submit" class="submit-button">Add Phone Number Here</button>
    <div v-if="showNoteSection" class="note-section">
      <textarea v-model="note" placeholder="Enter Note" class="note-field"></textarea>
    </div>
  </form>
</template>

<script>
import { ref } from 'vue';

export default {
  emits: ['addItem'],
  setup(_, { emit }) {
    // set variables
    const title = ref('');
    const note = ref('');
    const showNoteSection = ref(false);

    // validate phone number make sure there is a number and not random characters
    const validatePhoneNumber = (phoneNumber) => {
      const phoneRegex = /^\+?[1-9]\d{1,14}$/;
      return phoneRegex.test(phoneNumber);
    };

    // form submit - of phone is vaild than will sned a request to the API
    const submitForm = async () => {
      if (title.value.trim()) {
        // validate phone number
        if (!validatePhoneNumber(title.value)) {
          console.error('Invalid phone number format');
          return;
        }

        showNoteSection.value = true;

        try {
          // fetch request to the API
          const response = await fetch(`https://phonevalidation.abstractapi.com/v1/?api_key=&phone=${title.value}`);
          const data = await response.json();
          if (data.valid) {
            // if data valid than it will add to item
            const item = {
              title: title.value,
              code: data.country.code,
              name: data.country.name,
              prefix: data.country.prefix,
              location: data.location,
              carrier: data.carrier,
              note: note.value
            };
            emit('addItem', item);
          } else {
            console.error('Invalid phone number');
            emit('addItem', {title: title.value, note: note.value});
          }
        } catch (err) {
          console.error('API request failed', err);
          emit('addItem', {title: title.value, note: note.value});
        } finally {
          // reset the form to nothing
          title.value = '';
          note.value = '';
          showNoteSection.value = false;
        }
      }
    };

    return {title, note, showNoteSection, submitForm};
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: center;
}

.item-form {
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-field, .note-field {
  margin-right: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 100%;
  max-width: 500px;
}

.submit-button {
  padding: 8px 15px;
  border: none;
  border-radius: 3px;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
}

.note-section {
  margin-top: 10px;
}
</style>