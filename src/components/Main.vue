<template>
<div class="padre">
    <div class="tarjeta"> 
        <img src="../assets/International_Pokémon_logo.svg.webp" width="200">  
        <h1>Adivina quién es este pokémon?</h1>
        <img src="../assets/769px-Pokebola-pokeball-png-0.png" width="100"><br>
        
        <!-- Imagen del pokemon seleccionado-->
        <img v-if="pokemon!=null" :src="this.img" alt="" width="300">
        <h3>Seleccione su respuesta</h3>
        <!-- Nombre de todos los 5 pokemon  -->
        <select v-if="this.listPokemon.length > 4" name="" id="" v-model="selectedOption" >
            <option value="0" selected>Selecciona tu respuesta</option>
            <option value="1">{{this.listPokemon[0].name}}</option>
            <option value="2">{{this.listPokemon[1].name}}</option>
            <option value="3">{{this.listPokemon[2].name}}</option>
            <option value="4">{{this.listPokemon[3].name}}</option>
            <option value="5">{{this.listPokemon[4].name}}</option>
        </select>
    <div class="padre">
 
 <!-- Elementos visuales del chequeo de respuesta -->
    <div v-if="this.check" class="divc" >
         <span >Opcion Correcto</span>
    </div>
    <div v-else-if="this.check==null">
        <span></span></div>
    <div v-else class="divf">
        <span>Opcion Incorrecto</span>
    </div>
    </div>

    <!-- Botón para cargar nueva data -->
    <button id="next-button" @click="getobtener()"></button>
    </div>

 </div>

</template>



<script>
// Librería para trabajar con randoms
import {randomInt} from 'mathjs'
export default {
   
    data() {
        return {
            // Variables Globales
            pokemon:null,
            listPokemon:[],
            img: null,
            selectedOption:0,
            selectedPokemon:null,
            check:null
        }
    },
    watch:{
selectedOption(value){
          if(value!=0){
            if(value-1==this.selectedPokemon){
              console.log("Correcto")
              this.check=true;
            }
            else{
                console.log("Incorrecto")
               this.check=false;
            }
        }else{
            this.check=null
        }
        }
    } ,

    methods: {
         async getobtener(){
            //Limpieza de los componentes html para la carga de nueva data
            if(this.listPokemon.length>4){
                console.log('clean')
                this.listPokemon=[]
                this.pokemon=null;
                 this.selectedOption=0;
                  this.selectedPokemon=null;
            }
            
            //Extracción de data desde la API 
            for(var c=0; c<5; c++){
                var id= randomInt(1,500);
                this.listPokemon.push(await fetch('https://pokeapi.co/api/v2/pokemon/'+String(id)+'/').then(res =>res.json()))

            }

        
            this.selectedPokemon= randomInt(0,4);
            this.pokemon= this.listPokemon[this.selectedPokemon];
            // this.listPokemon.pop(selectedPokemon);
            this.img=this.pokemon.sprites.front_default;

            console.log('SelectedOption=' + this.selectedOption + '\nSelected Pokemon='+ this.selectedPokemon+1)
        }
        
    },
    created() {
         this.getobtener()
    }
    
}
</script>

<style>
.padre{
    display: flex;
    justify-content: center;
    color: aliceblue;
}

.divc{
background: rgb(30, 104, 42);
color: aliceblue;
margin: 10px;
padding: 10px;
border-radius: 10px;
}

.divf{
background: rgb(177, 0, 0);
color: aliceblue;
margin: 10px;
padding: 10px;
border-radius: 10px;
}
.tarjeta{
    background: cadetblue;
    padding: 10px;
    border-radius: 20px;
    width: 50%;
}

#next-button{
     width: 75px;
  height: 75px;
  background-color: #fff;
  border-radius: 48px;
  border: 2px solid #f2af29;
  transition: all 0.3s ease;
  display: inline;
  position: relative;
right: -20vw;

}
#next-button:hover {
  border-color: #14519f;
}

#next-button:hover::after {
  color: #14519f;
}

#next-button::after {
  content: "\279C";
  font-size: 18px;
  color: #f2af29;
  transition: all 0.3s ease;
}
select{
     font-family: Arial;
     font-size: large;
}
</style>