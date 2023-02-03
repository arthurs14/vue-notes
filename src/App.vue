<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref('');
const errorMessage = ref('');
const notes = ref([]);

const getRandomColor = () => {
  return `hsl(${Math.random() * 360}, 100%, 75%)`;
};

const toggleModal = () => {
  showModal.value = !showModal.value;

  if (!showModal.value) {
    errorMessage.value = '';
    newNote.value = '';
  }
};

const addNote = () => {
  if (newNote.value.length < 9) {
    return (errorMessage.value =
      'Your note needs to be 10 characters or more.');
  }

  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  // reset textarea and close modal
  newNote.value = '';
  errorMessage.value = '';
  showModal.value = !showModal.value;
};
</script>

<template>
  <main>
    <!-- Modal -->
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="toggleModal">Close</button>
      </div>
    </div>

    <!-- Main App -->
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="toggleModal">+</button>
      </header>
      <div class="card-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: #ffffff;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 0 20px 20px 0;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin: 15px 0 0 0;
}

.modal .close {
  background-color: rgb(193, 15, 15);
}

.modal p {
  color: rgb(193, 15, 15);
}
</style>
