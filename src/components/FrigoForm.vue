<script setup>
import { defineEmits, ref  } from "vue";
// -- donnée réactive pour la saisie du libellé
const nom = ref("");
const qte = ref(1);


// -- les events émis par le composant
const emit = defineEmits(["addAliment"]);

function handlerSubmit() {
  emit('addAliment', nom.value, qte.value);
  // réinitialise les champs du formulaire
  nom.value = "";
  qte.value = 1;
}
</script>


<template>     
  <form @submit.prevent="handlerSubmit">
  <input type="text" v-model="nom" placeholder="nouvel aliment" />
  <select v-model="qte">
      <option v-for="a in 30" :key="a" :value="a">{{ a }}</option>
  </select>
    <br>
  <input id="bouton" type="submit" value="valider" />
  </form>
</template>

<style scoped>
#bouton{
  background-color: #ffdca7;
  transition: background-color 0.3s ease;
  border-radius: 10%; /* Pour rendre le bouton rond */
  border: 3px solid #000000;
  border-width: 1px;
  cursor: pointer;
}

#bouton:hover {
      background-color:hsl(41, 100%, 92%) ;
      transition : 0.7s ;
  }
</style>
