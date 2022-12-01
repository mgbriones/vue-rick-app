<template lang="">
  <div class="table-responsive">
    <h2 class="subTitle">Lista de personajes</h2>

    <div class="boxInput">
      <input
        type="text"
        class="form-control text-red rounded"
        placeholder="Busca un personaje"
        @keyup="handlerInput()"
        v-model="text_search"
      />
    </div>

    <div class="tableInfo">
      <table v-if="listSearchCharacters.length" class="table table-dark">
        <thead>
          <tr>
            <th>#</th>
            <th>Picture</th>
            <th>Name</th>
            <th>Species</th>
            <th>Gender</th>
            <th>Status</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="characters in listSearchCharacters"
            :key="listCharacters.id"
          >
            <td class="text-muted">
              {{ characters.id }}
            </td>
            <td>
              <img
                :src="characters.image"
                style="width: 2rem"
                class="me-2 rounded-circle"
              />
            </td>
            <td>
              {{ characters.name }}
            </td>
            <td class="text-muted">
              {{ characters.species }}
            </td>
            <td class="text-muted">
              {{ characters.gender }}
            </td>
            <td v-if="characters.status == 'Alive'" class="text-success">
              {{ characters.status }}
            </td>

            <td v-else-if="characters.status == 'Dead'" class="text-danger">
              {{ characters.status }}
            </td>

            <td v-else class="text-muted">
              {{ characters.status }}
            </td>
          </tr>
        </tbody>
      </table>

      <h6 v-else-if="loadPage">No han encontrado resultados</h6>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let url = "https://rickandmortyapi.com/api/character/?page=";
let loadPage = false;
const listCharacters = ref([]);
const listSearchCharacters = ref([]);
let text_search = "";

const handlerInput = () => {
  listSearchCharacters.value = listCharacters.value.filter((character) =>
    character.name.toLowerCase().includes(text_search.toLowerCase())
  );
};

const handlerLoad = async () => {
  try {
    let response = await fetch(url);
    let resp_json = await response.json();
    listCharacters.value = listCharacters.value.concat(resp_json.results);
    listSearchCharacters.value = listCharacters.value.concat(resp_json.results);
    url = resp_json.info.next;

    for (let i = 2; i <= resp_json.info.pages; i++) {
      response = await fetch(url);
      resp_json = await response.json();

      listCharacters.value = listCharacters.value.concat(resp_json.results);
      listSearchCharacters.value = listCharacters.value.concat(
        resp_json.results
      );
      url = resp_json.info.next;
    }

    // la paguina esta cargada
    loadPage = true;
  } catch (err) {
    console.log(err);
  }
};

onMounted(() => {
  handlerLoad();
});
</script>

<style scoped>
div {
  color: white;
  background-color: black;
  padding: 2em;

  /* margin: 1em; */
}
input {
  margin: 0em;
  border-style: solid;
  border-width: 0.2em;
  border-color: rgb(171, 248, 244);
  align-items: center;
  width: 50%;
}

.boxInput {
  display: flex;
  justify-content: center;
  width: 50em;
}

.subTitle {
  text-align: center;
}

.table-responsive {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
