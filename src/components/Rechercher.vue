<script setup>
import { ref } from "vue";
// -- donnée réactive pour la saisie du libellé
const motsCle = ref("");
const emit = defineEmits(["recherche"]);
</script>

<template>
  <form @submit.prevent="$emit('recherche',motsCle )">
    <div>
      <input type="text" v-model="motsCle" placeholder="saisir l'aliment" />
    </div>
    <input id="bouton" type="submit" value="rechercher" />
    <div id="rechercheAliment"></div>
  </form>

</template>

<style scoped>
#bouton{
  background-color: #ffdca7;
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