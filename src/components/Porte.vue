<script setup>
// -- import de la fonction permettant de déclarer
//   une variable comme une varianel d'ETAT
import { reactive } from "vue";
import { ref } from 'vue';

let ouvrir = ref(false);

function ouvrirPorte() {
  ouvrir.value = true;
}
</script>


<template>
   
<body>
	<div class="porte" :class="{ 'visible': ouvrir }">
		<p @click="ouvrirPorte(null)" class="porte" id="porte"></p>
    </div>
</body>
</template>


<style scoped>
.porte p.porte{
    background: #ebb0e1; 
    position: absolute; top:0; left:0; 
    width: 450px; height:700px;
	display:inline-block; 
  
    transition: transform 1s ease-in-out;
    }

.porte.visible{
    width: 0;
    opacity: 0;
    margin: 0;
    
    /* perspective(1200px) translateY(0px) translateZ(0px) translateX(0px)*/
}
    /*text-align: left; vertical-align: top; */


</style>