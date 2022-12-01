<script setup>
import Test from "./components/test.vue";
// import { ref, onMounted } from 'vue'
import ListCharacters from "./components/ListCharacters.vue";

// const users = ref(null)
// onMounted( async () => {
//   const response = await fetch(url)
//   users.value = await response.json()
//   console.log(users.value.results[0].name)
// })
</script>

<template>
  <div class="algo">
    <h1>Rick App</h1>
  </div>

  <ListCharacters />
</template>

<style scoped>
.algo {
  color: white;

  background-color: black;
  padding: 2em;
  /* border-style: solid;
  border-width: 4em; */
  text-align: center;
  font-family: fantasy;
  /* font-size: 30px; */
  /* font-size: 200%; */
}

/* html, body {
     min-height: 100%;
} */


</style>
