<script setup>
import { ref } from "@vue/reactivity";

const atomicNumber = ref(0);
const mostrarMsg = ref(false);
const mostrarSalsa = ref(false);
const res = ref("")

const getSalsa = async () => {
  mostrarMsg.value = true;
  const salsa = await fetch(`https://api-doujinshi-download-production.up.railway.app/download-manga-pdf?atomicNumber=${atomicNumber.value}`);
  const salsaRes = await salsa.json()
  if (salsaRes.error) {
    return res.value = "El numero nuclear deber ser de 6 digitos"
  }
  res.value = salsaRes.res;
  mostrarSalsa.value = true
};
</script>

<template>
  <div>
    <h1 class="logo">Nhenta Srapper</h1>
  </div>
  <form @submit.prevent="" class="atomicNumber">
    <label for="atomic">Ingrese su codigo nuclear</label>
    <input type="number" id="atomic" v-model.trim="atomicNumber" />
    <button @click="getSalsa" v-if="!mostrarMsg && !mostrarSalsa" type="submit" class="btn-scap">
      Lets Go
    </button>
    <span v-if="mostrarMsg && !mostrarSalsa">Se esta recogiendo datos de su manga....</span>
    <span v-if="mostrarMsg && !mostrarSalsa">Esto puede tardar unos minutos....</span>
    <a v-if="mostrarSalsa" :href="`https://api-doujinshi-download-production.up.railway.app/${res}`" target="_blank">Listo aqui esta tu manga</a>
  </form>
</template>

<style scoped>
.logo {
  will-change: filter;
  transition: 200ms ease-out;
}
.logo:hover {
  filter: drop-shadow(0 0 1em #a2be27aa);
}
.atomicNumber {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#atomic {
  width: 200px;
  margin-top: 10px;
  padding: 5px;
}
.btn-scap {
  padding: 10px;
  margin-top: 10px;
  cursor: pointer;
}
span {
  margin-top: 10px;
}
</style>
