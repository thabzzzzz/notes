<script setup>
import { ref } from 'vue';
const modalOn = ref(false);

const toggleModal = ()=>{
  modalOn.value=ref(true);
}

const hideModal=()=>{
modalOn.value=false;
}

const newNote = ref('');
const notes = ref([]);

const getRandomColor =()=> {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  
}

const options = { day: 'numeric', month: 'long', year: 'numeric' };

const currentDate = new Date();
const formattedDate = currentDate.toLocaleDateString('en-GB', options);

const charError = ref(false);

const addNote = ()=>{

//valitaditon checks before main functionality
if(newNote.value.length>4){
  notes.value.push(
  {
    id:Math.floor(Math.random()*100000),
    text:newNote.value,
    date: formattedDate,
    background: getRandomColor()
  }
);
modalOn.value=false;
newNote.value='';
 charError.value=false;


}
else{
  charError.value=true;
}



}

</script>

<template>
  <main>
  <transition-group name="pop" appear>
    <div v-if="modalOn" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote" required></textarea>
        <p style="color: red;" v-if="charError">Minimum characters is 5</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="hideModal()">Close</button>
      </div>
     </div> 
  </transition-group>
    
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="toggleModal()">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id"       :style="{ backgroundColor: note.background }">
          <p class="main-text">
            
              {{ note.text }}
          </p>
          <p class="date">{{note.date}}</p>
        </div>
        
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto
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
    background-color: rgb(21,20,20);
    border-radius: 100%;
    color: white;
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
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.77);
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
    margin-top: 15px
  }

  .modal .close {
    background-color: rgb(193, 15, 15);
    margin-top: 7px;
  }

  .pop-enter-active,
.pop-leave-active {
  transition: transform 0.2s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.2s linear;
}

.pop-enter,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%) translateX(80%);
}

.pop-enter-to,
.pop-leave {
  opacity: 1;
  transform: scale(1) translateY(0) translateX(0);
}
</style>