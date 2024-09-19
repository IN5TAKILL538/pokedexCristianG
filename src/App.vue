<template>
  <div class="padre">
    <div class="header">
      <input
        type="text"
        id="pokemon"
        placeholder="Nombre del pokemon"
        v-model="nombrepokemon"
      />
      <button @click="listarPokemones()">Buscar</button>
    </div>
    <div class="container">
      <div class="imgpokemon">
        <h1 id="nompokemon">NOMBRE <br />{{ nombre }}</h1>

        <img :src="image" alt="" />
        <div class="alturapeso">
          <h1 class="altura">Altura: {{ altura }}m</h1>
          <h1 class="peso">Peso: {{ peso }}Kg</h1>
        </div>
      </div>

      <div class="datospokemon">
        <div id="numpokemon" class="estadisticas">
          # POKEDEX <br />
          {{ numpokedex }}
        </div>
        <br />
        <div id="tipo" class="estadisticas">
          Tipos<br /><br />
          {{ tipo1 }} <br />
          {{ tipo2 }}
        </div>
        <br />
        <div id="debilidad" class="estadisticas">Debilidades<br />{{}}</div>
        <br />
      </div>
      <div class="estadspokemon">
        <h1>ESTADISTICAS</h1>
        <div id="hp" class="estadisticas">HP <br />{{ statshp }}/252</div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barrahp" />
        </div>
        <br />
        <div id="attack" class="estadisticas">
          ATAQUE <br />{{ statsatk }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraatk" />
        </div>
        <br />
        <div id="defense" class="estadisticas">
          DEFENSA <br />{{ statsdf }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barradf" />
        </div>
        <br />
        <div id="spattack" class="estadisticas">
          ATAQUE ESPECIAL <br />{{ statsatksp }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraatksp" />
        </div>
        <br />
        <div id="spdefense" class="estadisticas">
          DEFENSA ESPECIAL <br />{{ statsdfsp }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barradfsp" />
        </div>
        <br />
        <div id="speed" class="estadisticas">
          VELOCIDAD <br />{{ statsspeed }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraspeed" />
        </div>
        <br />
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

let nombrepokemon = "";
let nombre = ref("");
let image = ref("");
let numpokedex = ref("");
let statshp = ref("");
let statsatk = ref("");
let statsdf = ref("");
let statsatksp = ref("");
let statsdfsp = ref("");
let statsspeed = ref("");
let tipo1 = ref("");
let tipo2 = ref("");
let altura = ref("");
let peso = ref("");
let debilidades = ref([]);

async function obtenerDebilidades(tipos) {
  const debilidadesSet = new Set();

  for (const tipo of tipos) {
    const response = await axios.get(`https://pokeapi.co/api/v2/type/${tipo}`);
    const data1 = response.data;

    // Agregar debilidades de este tipo
    for (const debilidad of data1.damage_relations.double_damage_from) {
      debilidadesSet.add(debilidad.name);
    }
  }
  console.log(debilidadesSet + "debilidades");
  return Array.from(debilidadesSet);
}
async function listarPokemones() {
  if (nombrepokemon == "" || nombrepokemon == null) {
    document.body.style.backgroundImage =
      "url('https://nintendo.pe/wp-content/uploads/2016/05/HddtBOT-copia.jpg')";
  } else {
    

    let url = "https://pokeapi.co/api/v2/pokemon/" + nombrepokemon;

    let { data } = await axios.get(url);
    console.log(data);
    nombre.value = data.name;
    numpokedex.value = data.id;
    image.value = data.sprites.front_default;
    statshp.value = data.stats[0].base_stat;
    console.log(statshp);

    statsatk.value = data.stats[1].base_stat;
    statsdf.value = data.stats[2].base_stat;
    statsatksp.value = data.stats[3].base_stat;
    statsdfsp.value = data.stats[4].base_stat;
    statsspeed.value = data.stats[5].base_stat;

    if (data.types.length == 2) {
      tipo1.value = data.types[0].type.name;

      tipo2.value = data.types[1].type.name;
    } else {
      tipo1.value = data.types[0].type.name;

      tipo2.value = "-";
    }
    altura.value = data.height / 10;
    peso.value = data.weight / 10;
    const tipos = data.types.map((tipo) => tipo.type.name);
    debilidades.value = await obtenerDebilidades(tipos);
  }
}
</script>


<style>
.contenedorbarras {
  width: 256px;

  height: 10px;
  border: 2px solid black;
}
.barrastats {
  height: 10px;
  color: rgb(11, 11, 49);
  background-color: blue;
}
.padre {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
}
.header {
  background-image: url(https://cdn.alfabetajuega.com/alfabetajuega/2019/03/iniciales-1.jpg);
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;

  display: flex;
  justify-content: center;
  align-items: end;
  padding-bottom: 20px;

  height: 200px;
}
.container {
  background-image: url(https://e1.pxfuel.com/desktop-wallpaper/269/183/desktop-wallpaper-rotom-pokedex-backgrounds-pokedex.jpg);
  background-size: cover;
  background-position: center;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: center;
  min-height: 100vh;
  max-height: auto;
}

#nompokemon {
  text-align: center;
}

.imgpokemon {
  display: flex;
  flex-direction: column;
  width: 400px;
  border-radius: 1px;
  border: 2px solid black;
  margin: 10px;
}
.alturapeso {
  display: flex;
  justify-content: space-evenly;
}

.datospokemon {
  display: flex;
  flex-direction: column;
  width: 400px;
  border: 2px solid black;
  margin: 10px;
  justify-content: space-evenly;
  align-items: center;
}
#tipo {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.estadspokemon {
  display: flex;
  flex-direction: column;
  width: 400px;
  justify-content: center;
  align-items: center;
  border: 2px solid black;
  margin: 10px;
}
.estadisticas {
  text-align: center;
}

@import "./pokemon_types.scss";
</style>
