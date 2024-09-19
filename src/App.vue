

<template>
  <div id="app">
    <h1>Initiative Tracker</h1>
    <ul>
      <li v-for="(character, index) in sortedCharacters" :key="character.name" :class="{ active: index === currentTurnIndex }">
        <div class="info">
          <span>{{ character.name }}</span>
          <span>{{ character.initiative }}</span>
        </div>
        <div class="buttons">
        <button @click="moveUp(index)" :disabled="index === 0" class="icon-btn">↑</button>
        <button @click="moveDown(index)" :disabled="index >= sortedCharacters.length - 1" class="icon-btn">↓</button>
        <button @click="removeCharacter(index)" class="delete-btn">X</button>
        </div>
      </li>
    </ul>
    <button @click="showAddCharacterModal">Add character</button>
    <button @click="nextCharacter">Next character</button>
     <!-- Popup Modal for adding a new character -->
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <h2>Add new character</h2>
        <form @submit.prevent="addCharacter">
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="newCharacterName" required>
          <label for="initiative">Initiative:</label>
          <input type="number" id="initiative" v-model="newCharacterInitiative" required>
          <button type="submit">Add character"</button>
          <button type="button" @click="showModal = false">Cancel</button>
        
        </form>
      </div>

    </div>

  </div>

</template>

<!-- <script lang="ts">
export default {
  data() {
    return {
      // Character array directly in the data function
      characters: [
        { name: "Aragorn", initiative: 14 },
        { name: "Gimli", initiative: 12 },
        { name: "Legolas", initiative: 18 },
        { name: "Gandalf", initiative: 16 }
      ]
    };
  },
  computed: {
    // Computed property to sort characters by initiative
    sortedCharacters() {
      return [...this.characters].sort((a, b) => b.initiative - a.initiative);
    }
  }
};
</script> -->

<script setup lang="ts">
import { ref , computed } from "vue";
const characters = ref( [
{ name: "Aragorn", initiative: 14 },
  { name: "Gimli", initiative: 12 },
  { name: "Legolas", initiative: 18 },
  { name: "Gandalf", initiative: 16 }
]);

const currentTurnIndex = ref(0);

const sortedCharacters = computed(() =>
  [...characters.value].sort((a, b) => b.initiative - a.initiative)
);

// Modal visibility and form inputs
const showModal = ref(false);
const newCharacterName = ref("");
const newCharacterInitiative = ref<number | null>(null);

// adds a new character to the list when button is pressed and then updated sortedCharacters
function addCharacter() {
  if (newCharacterName.value && newCharacterInitiative.value !== null){
    characters.value.push({
      name: newCharacterName.value,
      initiative: newCharacterInitiative.value,
    });
  }
  //close modal
  showModal.value = false;

}

//function for resetting the values of the modal and showing the modal when clicking the button 
function showAddCharacterModal() {
  newCharacterName.value="";
  newCharacterInitiative.value=null;
  showModal.value = true;
  }

function removeCharacter(index: number){
  characters.value.splice(index, 1);
}

//function called nextCharacter that increments currentTurnIndex
function nextCharacter() {
  if (currentTurnIndex.value < sortedCharacters.value.length -1){
    currentTurnIndex.value++;
  } else {
    currentTurnIndex.value = 0;
  }
}

function moveUp(index: number){
  if (index > 0){
    const currentCharacter = sortedCharacters.value[index];
    const aboveCharacter = sortedCharacters.value[index-1];
    // set currenctCharacter initiative to abovecharacter initiativ +1
    currentCharacter.initiative = aboveCharacter.initiative + 1;
    }
}

function moveDown(index: number){
  if (index < sortedCharacters.value.length - 2){
    const currentCharacter = sortedCharacters.value[index];
    const belowCharacter = sortedCharacters.value[index+ 2];
    // set currenctCharacter initiative to belowcharacter initiativ +1
    currentCharacter.initiative = belowCharacter.initiative + 1;
    }
  if (index == sortedCharacters.value.length - 2){
    const currentCharacter = sortedCharacters.value[index];
    const belowCharacter = sortedCharacters.value[index+1];
    // set currenctCharacter initiative to belowcharacter initiativ-1
    currentCharacter.initiative = belowCharacter.initiative - 1;
    
    }
  }


</script>

<style>
/* Style the entire window */
html, body {
  height: 100%;
  margin: 0;
  background-color: #004a39;
}
</style>

<style scoped>


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  padding-top: 20px;
}

h1 {
  color: burlywood;
  font-size: 2em;
  margin-bottom: 20px;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}
/* Remove bullet points from the unordered list */
ul {
  list-style: none;
  padding: 0;
  margin: 0;
  /*color of text*/
  color: #7cffa1;

  }
  .modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  width: 300px;
  text-align: center;
}

label {
  display: block;
  margin: 10px 0 5px;
}

input {
  padding: 5px;
  width: 100%;
  margin-bottom: 15px;
  box-sizing: border-box;
}

/* Delete button styling */
.delete-btn {
  background-color: transparent;
  border: none;
  color: red;
  cursor: pointer;
  font-size: 1.2em;
}

.active {
  font-weight: bold;
  color: gold;
}

.icon-btn {
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 1em; /* Adjust size */
  color: gray;    /* Subtle color */
  padding: 0.2em; /* Smaller padding */
}

.icon-btn:disabled {
  color: lightgray; /* Lighter color when disabled */
}

.icon-btn:hover {
  color: black; /* Darker color on hover */
}

li {
  display: flex;
  justify-content: space-between; /* Distribute space evenly */
  align-items: center; /* Vertically center content */
  padding: 0.3em 0; /* Add some spacing between items */
  height: 1.5em;
}

li .info {
  flex: 1; /* Make the name and initiative take most of the space */
  display: flex;
  justify-content: space-between; /* Space between name and initiative */
  padding-left: 20%;
  padding-right: 20%;
  
}

li .buttons {
  display: flex;
  gap: 0.5em; /* Space between buttons */
  align-items: center;
  padding-bottom: 5%;
  padding-right: 5%;
}

.info span{
  line-height: 1.5;
}

.icon-btn, .delete-btn {
  height: 1.5em; /* Adjust button height to match text */
  line-height: 1.5em; /* Ensure vertical alignment inside buttons */
  padding: 0; /* Remove padding for consistent size */
  display: flex;
  justify-content: center;
  align-items: center; /* Center content inside buttons */
}

</style>