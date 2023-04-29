<script setup>
import { ref, effect } from 'vue';

const showModal = ref(false);
const notes = ref([]);
const noteContent = ref('');
const error = ref(false);

effect(() => {
  if (!(noteContent.value.length === 0 || noteContent.value.length > 160))
    error.value = false;

  // console.log('------------>', noteContent.value);
}, noteContent);

const getRandomColor = () => {
  return 'hsl(' + Math.random() * 360 + ', 100%, 75%)';
};

const noteContentIsValid = () => {
  return noteContent.value.length === 0 || noteContent.value.length > 160;
};

const addNote = () => {
  if (noteContentIsValid()) {
    error.value = true;
    return;
  }

  const lastNote = notes.value[notes.value.length - 1];

  notes.value.push({
    id: lastNote?.id + 1 || 1,
    text: noteContent.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  });

  showModal.value = false;
  noteContent.value = '';
};

const toggleModal = () => {
  showModal.value = !showModal.value;
};
</script>

<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model="noteContent"
          name="note"
          id="note"
          cols="30"
          rows="10"
          :style="error && { border: '2px solid #c10f0f' }"
        ></textarea>
        <button @click="addNote()">Add Note</button>
        <button class="close" @click="toggleModal()">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="toggleModal()">+</button>
      </header>

      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  width: 100vw;
  min-height: 100vh;
}

.container {
  max-width: 1024px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 {
  font-size: 75px;
  font-weight: bold;
  margin-bottom: 25px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: #151414;
  border-radius: 100%;
  color: white;
  font-size: 24px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: #edb62c;
  border-radius: 15px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.card .main-text {
  font-weight: bold;
}

.date {
  font-size: 12.5px;
  font-style: italic;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: #000000c4;
  z-index: 7;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal textarea {
  border-radius: 5px;
  resize: none;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #8a2be2;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  background-color: #c10f0f;
  margin-top: 7px;
}
</style>
