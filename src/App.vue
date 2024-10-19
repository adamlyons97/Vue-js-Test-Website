<script setup>
import { ref,onMounted } from 'vue';

let names = ref([])
let name = ref('')

async function addName(e){
  e.preventDefault()
  console.log(name.value)
  names.value.push(name.value)
  let response = await fetch("https://narwhal-direct-briefly.ngrok-free.app/Home",{
    method : 'POST',
    headers :{
      'Content-Type': 'application/json',
      'ngrok-skip-browser-warning':'true'
    },
    body: JSON.stringify({id:0,username:name.value})
  })

  let data = await response.json()

  names.value = data

}
function clearNames(){
  names.value = []
}

async function retrieveNamesFromServer(){
  let response = await fetch("https://narwhal-direct-briefly.ngrok-free.app/Home",{
    method : 'GET',
    headers :{
      'Content-Type': 'application/json',
      'ngrok-skip-browser-warning':'true'
    }
  })
  let data = await response.json()
  names.value = data
}


onMounted(async ()=>{
  await retrieveNamesFromServer()
})
</script>

<template>
<form @submit="addName" >
  <label for="">
    Name: 
    <input v-model="name" type="text">
  </label>
</form>
<button @click="clearNames">Clear</button>
<ul>
  <li v-for="name in names">{{ name.username }}</li>
</ul>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
