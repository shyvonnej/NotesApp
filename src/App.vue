<script setup>
import { ref } from "vue"; //state
const showModal = ref(false) //state name = showModal
const newNote = ref("")
const notes = ref([]);
const errorMsg = ref("")

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return errorMsg.value = "Note needs to be more than 10 characters."
  }
  notes.value.push({
    id: Math.floor(Math.random() * 10000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
  newNote.value = ""
  errorMsg.value = ""
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="notes" id="notes" cols="30" rows="10"></textarea>
        <p v-if="errorMsg">{{ errorMsg }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="card-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100ww;
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
  background-color: antiquewhite;
  border-radius: 100%;
  font: 20px;
}

.card {
  width: 255px;
  height: 255px;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  color: black;
  font-size: 12px;
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
  background-color: rbga(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: darkslategrey;
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
  background-color: whitesmoke;
  cursor: pointer;
  margin-top: 20px;
}

.modal .close {
  background-color: rgb(228, 117, 117);
  margin-top: 7px;
}

.main-text {
  color: black;
  font-size: 15px;
  font-weight: 400;
}

.modal p{
  color: red;
}
</style>