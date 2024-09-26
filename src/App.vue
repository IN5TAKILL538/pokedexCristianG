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
    <div class="container" id="container">
      <div class="imgpokemon" id="imgpokemon" >
        <h1 id="nompokemon">NOMBRE <br />{{ nombre }}</h1>

        <img :src="image" alt="" id="imagenpoke" :style="{background: obtenerColorPorTipo(tipo1)}"/>
        <div class="alturapeso">
          <h1 class="altura">Altura: {{ altura }}m</h1>
          <h1 class="peso">Peso: {{ peso }}Kg</h1>
        </div>
      </div>

      <div class="datospokemon" id="datospokemon">
        <div id="numpokemon" class="estadisticas">
          # POKEDEX <br />
          {{ numpokedex }}
        </div>
        <br />
        <div id="tipo" class="estadisticas" >
          Tipos<br /><br />
          <button id="tipo1" class="btntipo" :style="{ background: obtenerColorPorTipo(tipo1) }">{{ tipo1 }}</button> 
          <br>
          <button id="tipo2" class="btntipo" :style="{ background: obtenerColorPorTipo(tipo2) }">{{ tipo2 }}</button>
        </div>
        <br />
        <div id="debilidad" class="estadisticas">
      <button class="containerdebilidad" v-for="(debilidad, index) in debilidades" :key="index" :style="{ background: obtenerColorPorTipo(debilidad) }">
        DEBILIDADES{{ debilidad }}
      </button></div>
        <br />
      </div>
      <div class="estadspokemon" id="estadspokemon">
        <h1>ESTADISTICAS</h1>
        <div id="hp" class="estadisticas">HP <br />{{ statshp }}/252</div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barrahp"  :style="{ width: (statshp / 252 * 100) + '%' }"/>
        </div>
        <br />
        <div id="attack" class="estadisticas">
          ATAQUE <br />{{ statsatk }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraatk" :style="{ width: (statsatk / 252 * 100) + '%' }"/>
        </div>
        <br />
        <div id="defense" class="estadisticas">
          DEFENSA <br />{{ statsdf }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barradf" :style="{ width: (statsdf / 252 * 100) + '%' }"/>
        </div>
        <br />
        <div id="spattack" class="estadisticas">
          ATAQUE ESPECIAL <br />{{ statsatksp }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraatksp" :style="{ width: (statsatksp / 252 * 100) + '%' }"/>
        </div>
        <br />
        <div id="spdefense" class="estadisticas">
          DEFENSA ESPECIAL <br />{{ statsdfsp }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barradfsp" :style="{ width: (statsdfsp / 252 * 100) + '%' }"/>
        </div>
        <br />
        <div id="speed" class="estadisticas">
          VELOCIDAD <br />{{ statsspeed }}/252
        </div>

        <div class="contenedorbarras">
          <hr class="barrastats" id="barraspeed" :style="{ width: (statsspeed / 252 * 100) + '%' }"/>
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
function obtenerColorPorTipo(tipo) {
    const colores = {
        normal: '#A8A878',
        fire: '#F08030',
        water: '#6890F0',
        grass: '#78C850',
        electric: '#F8D030',
        ice: '#98D8D8',
        fighting: '#C03028',
        poison: '#A040A0',
        ground: '#E0C068',
        flying: '#A890F0',
        psychic: '#F85888',
        bug: '#A8B820',
        rock: '#B8A038',
        ghost: '#705898',
        dragon: '#7038F8',
        dark: '#705848',
        steel: '#B8B8D0',
        fairy: '#F0B6BC',
    };

    return colores[tipo] || '#FFFFFF'; // Color por defecto
}

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
  console.log(debilidadesSet);
  return Array.from(debilidadesSet);
}
async function listarPokemones() {
  if (nombrepokemon == "" || nombrepokemon == null ) {
    
    document.getElementById("container").style.backgroundPosition="center"
     document.getElementById("container").style.backgroundSize="cover"
    document.getElementById("imgpokemon").style.display="none"
    document.getElementById("datospokemon").style.display="none"
    document.getElementById("estadspokemon").style.display="none"

  } else {
    document.getElementById("container").style.display="flex"
     
    document.getElementById("imgpokemon").style.display="flex"
    document.getElementById("datospokemon").style.display="flex"
    document.getElementById("estadspokemon").style.display="flex"
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

      tipo2.value = "";
    }
    altura.value = data.height / 10;
    peso.value = data.weight / 10;
    const tipos = data.types.map((tipo) => tipo.type.name);
    debilidades.value = await obtenerDebilidades(tipos);
  }
}
</script>


<style>
@media (min-width: 1000px){
  body{
    margin-right: 150px;
    margin-left: 150px;
  }
}
body{
  background-image: url(https://i.pinimg.com/564x/49/68/12/496812978f3eacb8871b88f4008e3d22.jpg);
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover
;
   border: 2px solid black;
  
}
.containerdebilidad{
  background-color: rgb(5, 0, 0);
  border-radius: 6px;
  border: 1px;
}
.btntipo{
  background-color: rgb(5, 0, 0);
  border-radius: 6px;
  border: 1px;

}

.contenedorbarras {
  width: 256px;

  height: 10px;
  border: 1px solid black;
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
  background-image: url(https://i.pinimg.com/originals/fe/99/25/fe9925baad42556c34a7a13bbf4fa6bf.gif);
  opacity: 0.8;
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
  
  background-size: cover;
  background-position: center;
  display: none;
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
#imagenpoke{
  border-radius: 1px;
  border: 2px solid black;
  border-radius: 10px;
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
  flex-direction: column;
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
