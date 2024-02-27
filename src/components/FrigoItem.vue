<script setup>
defineProps(["aliment"]);
defineEmits(["supprimer","ajouterUn","enleverUn"])
</script>

<template>

	<li>
    {{ aliment.pourAfficher() }}
         <button @click="$emit('supprimer', aliment.id)" id="bouton">
        <img src="./icons/poubelle.png" /></button>
        <span class="espace"></span>
        <button @click="$emit('ajouterUn', aliment)" class="boutonQtt" id="boutonPlus">+</button>
        <span class="espace"></span>
        <button @click="$emit('enleverUn', aliment)" class="boutonQtt" id="boutonMoins">-</button>
<br><br>
    </li>
            
       
       
</template>

<style>
  ul {
  /* afin de retirer les puces de la liste */
  list-style-type: none;
}

.espace {
  margin-right: 5px; /* Ajustez la marge selon vos besoins */
}

#bouton {
  background-color: rgba(0, 0, 0, 0);
  border-left-style: none;
  border-style: none;
  cursor: pointer;
}

#boutonPlus,
#boutonMoins{
  background-color: #ffdca7;
  border-radius: 30%; /* Pour rendre le bouton rond */
  transition: background-color 0.3s ease;
  cursor: pointer;
  border-width: 1px;
}

#boutonPlus:hover,
#boutonMoins:hover {
      background-color:hsl(41, 100%, 92%) ;
      transition : 0.7s ;
  }
    </style>

