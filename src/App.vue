<script setup>
import { ref } from 'vue'

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMessages = ref('')

const uid = function(){
        return Date.now().toString(36) + Math.random().toString(36).substr(2);
    }
function getRandomColor(){
    return `hsla(${~~(360 * Math.random())}, 70%,  72%, 0.8)`
}

const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessages.value = 'Please enter a note'
  }
  notes.value.push({
    id: uid(),
    text: newNote.value,
    date: new Date().toLocaleString(),
    backgroundColor:getRandomColor()
  });
  showModal.value = false 
  newNote.value = ''
  errorMessages.value = ''
}


</script>

<template>
  <main>

    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p>{{ errorMessages }}</p>
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
        <div 
        v-for="note in notes" 
        class="card" 
        :style="{backgroundColor: note.backgroundColor}"
        :key="note.id"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{note.date}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height:100vh;
  width:100vw;
}

.container {
  max-width: 1000px;
  padding:10px;
  margin: 0 auto;
}

header{
  display:flex;
  justify-content:space-between;
  align-items:center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  color: #333;
  font-size: 75px;
}

header button {
  background-color: #333;
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  font-size: 30px;
  cursor: pointer;
}

.card{
  width:225px;
  height:225px;
  background-color: rgb(255, 157, 82);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12px;
  font-weight:bold;
  color: #000000;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
  width: 750px;
  border-radius: 15px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.modal button {
  background-color: #cd1bda;
  color: #fff;
  border: none;
  width: 100%;
  height: 50px;
  font-size: 30px;
  margin-top:10px;
  cursor: pointer;
}

.modal .close {
  background-color: #d60e0e;
  color: #fff;
  border: none;
  margin-top:10px;
  font-size: 30px;
  cursor: pointer;
}

.modal p {
  color:#d60e0e;
}
</style>