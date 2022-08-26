<script setup>
import { ref, watch, onMounted, onUpdated } from 'vue'
import M from 'materialize-css'

const monsters = ref(null)
const monsterList = ref(null)
const filterElement = ref(null)

async function getMonsters() {
  monsters.value = null
  const res = await fetch('https://mhw-db.com/monsters')
  monsters.value = await res.json()
  monsterList.value = monsters.value
}

onMounted(()=>{
  M.AutoInit()
  getMonsters()
})

onUpdated(() => {
  monsters.value = monsters.value.sort((a,b) => a.name.localeCompare(b.name)) 
})

watch(filterElement, (newFilter)=>{
  if(!newFilter){
    monsters.value = monsterList.value
  }
  else{
    monsters.value = monsterList.value.filter((elem)=>elem.elements.includes(newFilter))
  }
})

function changeFilter(newFilter){
  filterElement = newFilter
}



</script>

<template>

  <div class="container">
    <div class="section">
      <h4 class="brown-text">Monster Hunter World - Monster list</h4>
    </div>
    <div class="divider"></div>
    <div class="section">
      <div class="row">
        <div class="col s1"><i class="material-icons">filter_list</i></div>
        <div class="col s2"><span>Filter list by Element: </span></div>
        <div class="col s9">
          <div class="btn-small orange darken-2 white-text tooltipped" data-position="bottom" data-tooltip="Fire" @click="filterElement = 'fire'"><i class="material-icons">local_fire_department</i></div>&nbsp;
          <div class="btn-small blue darken-2 white-text tooltipped" data-position="bottom" data-tooltip="Water" @click="filterElement = 'water'"><i class="material-icons">water</i></div>&nbsp;
          <div class="btn-small white blue-text text-darken-4 tooltipped" data-position="bottom" data-tooltip="Ice" @click="filterElement = 'ice'"><i class="material-icons">severe_cold</i></div>&nbsp;
          <div class="btn-small yellow darken-2 white-text tooltipped" data-position="bottom" data-tooltip="Thunder" @click="filterElement = 'thunder'"><i class="material-icons">flash_on</i></div>&nbsp;
          <div class="btn-small purple darken-2 white-text tooltipped" data-position="bottom" data-tooltip="Dragon" @click="filterElement = 'dragon'"><i class="material-icons">restaurant</i></div>&nbsp;
          <div class="btn-small red darken-2 white-text tooltipped" data-position="bottom" data-tooltip="No filter" @click="filterElement = null"><i class="material-icons">filter_alt_off</i></div>
        </div>
      </div>
    </div>
    <div class="divider"></div>
    <div class="section">
      <br>
      <div v-if="monsters">
        <div class="row">
            <div class="col s2 orange-text text-darken-4">Name</div>
            <div class="col s2 orange-text text-darken-4">Species</div>
            <div class="col s3 orange-text text-darken-4">Elements</div>
            <div class="col s5 orange-text text-darken-4">Description</div>
            
        </div>
        <div v-for="monster in monsters" :key="monster.id">
          
          <div class="row">
            <div class="col s2">{{ monster.name }}</div>
            <div class="col s2">{{ monster.species.charAt(0).toUpperCase() + monster.species.slice(1) }}</div>
            <div class="col s3"> 
              <span v-if="monster.elements.length > 0" v-for="element in monster.elements"> 
                {{ ' | ' + element.charAt(0).toUpperCase() + element.slice(1) + ' | ' }} 
              </span>
              <span v-else class="grey-text text-lighten-1"> None</span>
            </div>
            <div class="col s5">{{ monster.description }}</div>
          </div>

        </div>
      </div>
      <h3 v-else>Cargando...</h3>

      
    </div>
  </div>

</template>



<style scoped>

@font-face {
  font-family: "Aboreto";
  src: local("Aboreto"),
   url(./fonts/Aboreto-Regular.ttf) format("truetype");
}
@font-face {
  font-family: "Montserrat";
  src: local("Montserrat"),
   url(./fonts/Montserrat-VariableFont_wght.ttf) format("truetype");
}

*{
  font-family: Montserrat;
}

h1,h2,h3,h4,h5{
  font-family: Aboreto;
}

</style>
