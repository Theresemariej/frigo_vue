<script setup>
import { reactive, onMounted } from "vue";
import FrigoItem from "./FrigoItem.vue";
import FrigoForm from "./FrigoForm.vue";
import Rechercher from "./Rechercher.vue";
import Aliment from "../Aliment.js";

const listeC = reactive([]);
const listeR = reactive([]);

// -- l'url de l'API
const url = "https://webmmi.iut-tlse3.fr/~pecatte/frigo/public/14/produits";


function handlerDelete(id) {
  console.log(id);
  const fetchOptions = {
    method: "DELETE",
  };
  // -- l'id de l'aliment à supprimer doit être ajouté à la fin de l'url
  fetch(url + "/" + id, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      getTodos();
    })
    .catch((error) => console.log(error));
}


   function handlerAdd(nom, qte) {
  // -- il faut créer un nouvel aliment instance de la classe

  console.log(nom, qte);
 
  let myHeaders = new Headers();
  let photo = "https://webmmi.iut-tlse3.fr//~pecatte//frigo//public//images// "+nom;
  myHeaders.append("Content-Type", "application/json");

  const fetchOptions = {
    method: "POST",
    headers: myHeaders,
    body: JSON.stringify({nom:nom, qte:qte, photo:photo}),
  };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      getTodos();
    })
    .catch((error) => console.log(error));
}

function handlerPlus(aliment) {
  console.log(aliment); 
  let id = aliment.id;
  let nom = aliment.nom;
  let photo = aliment.photo;
  aliment.qte=aliment.qte+1;
  let qte = aliment.qte; 


  let myHeaders = new Headers(); 
  myHeaders.append("Content-Type", "application/json"); 
  const fetchOptions = {
    method: "PUT",
    headers : myHeaders,
    body: JSON.stringify({id: id, nom: nom, qte: qte, photo:  photo}),
  };

  fetch(url , fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
     getTodos(); 
  })
    .catch((error) => console.log(error));
}

function handlerMoins(aliment) {
  console.log(aliment); 
  let id = aliment.id;
  let nom = aliment.nom;
  let photo = aliment.photo;

  if(aliment.qte>0){aliment.qte=aliment.qte-1;}
  
  let qte = aliment.qte; 


  let myHeaders = new Headers(); 
  myHeaders.append("Content-Type", "application/json"); 

  if(qte==0){handlerDelete(id)}
  
  const fetchOptions = {
    method: "PUT",
    headers : myHeaders,
    body: JSON.stringify({id: id, nom: nom, qte: qte, photo:  photo}),
  };

  fetch(url , fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
     getTodos(); 
  })
    .catch((error) => console.log(error));
}


function handlerRecherche(mot){
    const urlPers = "https://webmmi.iut-tlse3.fr/~pecatte/frigo/public/14/produits?search=";
    const fetchOptions = { method: "GET" };

    fetch(urlPers+mot, fetchOptions)
    .then((response) => {
        return response.json();
    })
    .then((dataJSON) => {
      listeR.splice(0, listeR.length);
      dataJSON.forEach((v) => listeR.push(new Aliment(v.id, v.nom, v.qte, v.photo)));

        document.getElementById("rechercheAliment").innerHTML = ""
     
        document.getElementById("rechercheAliment").innerHTML += "<ul id='liste'>";
      // on insère de l'html pour créer la liste de resultat de la recherche//
      for (let l of listeR) {
        //on reccupère les attributs des aliments et on les incère dans l'html //
        document.getElementById("rechercheAliment").innerHTML +=
          "<li>" +
          l.nom+
          "</li>";
      }
      // on ferme la liste //
      document.getElementById("rechercheAliment").innerHTML += "</ul>";
    })
    .catch((error) => console.log(error));
  }

function getTodos() {
  const fetchOptions = { method: "GET" };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      // -- vider la liste des Aliments
      listeC.splice(0, listeC.length);
      /* pour chaque donnée renvoyée par l'API, on crée un objet instance de la classe Aliment et on l'ajoute dans la liste listeC*/
      dataJSON.forEach((v) => listeC.push(new Aliment(v.id, v.nom, v.qte, v.photo)));
    })
    .catch((error) => console.log(error));
}

onMounted(() => {
  getTodos();
});

</script>
<template>

  <ul id="fond"> 
    <div id="liste">
    
      <FrigoItem
      v-for="aliment of listeC"
      :key="aliment.id"
      :aliment="aliment"
      @supprimer="handlerDelete"
      @ajouterUn="handlerPlus"
      @enleverUn="handlerMoins"/>
    </div>
  </ul>	

  <div id="formulaire">
    <h2>Ajoutez un aliment</h2>
  <FrigoForm @addAliment="handlerAdd"></FrigoForm>
  </div>

<div id="chercher">
  <h2>Recherchez un aliment</h2>
  <Rechercher 
  @recherche="handlerRecherche">
 </Rechercher>

</div> 
  </template>

<style scoped>


#fond{
  background: url("./icons/image_de_fond.jpg") no-repeat;
  position: absolute; top:0; left:0; 
  width: 50vw; 
  height:100vh;
	display:inline-block;}

  #liste{
  list-style-type: none;
  padding: 0;
  margin: 90px 0px;
}

#formulaire{
  position: relative;
  top:50%;
  left:50%;
  margin: 10px 20px 10px 20px;
}

#chercher{
  position: relative;
  top:50%;
  left:50%;
  margin: 10px 20px 10px 20px;
}
</style>