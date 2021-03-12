<template>
  <div id="app">
   <div class="stats">
        <h1 class="stats__title">Secret Santa</h1>
        <h4 class="stats__title1">Ajouter des participants:</h4>
      </div>
      <person @person_added="delete_person(index)" :key="index" v-for="(data,index) in list_afficher" :data="data"> </person>
      
      <div class="add_person">
        <div class="add_person__input ">
            <input placeholder="Nom Prenom" type="text" v-model="new_person.nom" @keyup.enter="add_person">
            <input placeholder="Boite email" type="text" v-model="new_person.email"  @keyup.enter="add_person" >
        </div>
        <button class="add_person__btn" title="ajouter" @click="add_person">➕</button> 
      </div>
  
        <!-- liste noire -->
      <div class="black_list">
        <h3><b>Liste Noire</b></h3>
        <select v-model="new_black_list.nom1_black_list">
           <option></option>
           <option :key="index" v-for="(data,index)  in list_afficher" >{{data.nom}}</option>
        </select>
  
       <div class="texte_black_liste">Ne peut pas faire de cadeau à:</div>
  
       <select v-model="new_black_list.nom2_black_list">
           <option></option>
           <option :key="index" v-for="(data,index)  in list_afficher" >{{data.nom}}</option>
        </select>
        
        <button class="black_button" title='ajouter' @click="add_black_list"> ➕ </button> 
            <div class="black_list_afficher" :key="index" v-for="(data,index) in black_list "><span class="span_nom">{{data.nom1_black_list}}</span> Ne peut pas faire de cadeau à <span class="span_nom">{{data.nom2_black_list}}</span> 
                <button class="person_added" @click="delete_black_list(index)" > ❌ </button> 
           </div> 
      </div> <br>
       <!-- FIN liste noire -->
  
  
      <button class="tirage" title="tirage" @click="tirage_liste" >Faire le tirage au sort</button>

      <div class="res_tirage" :key="index" v-for="(data,index)  in list_santa"><span class="span_nom">{{data.nom1}}</span> <b> fait un cadeau à </b> <span class="span_nom">{{data.nom2}}</span></div> 
  
  
  
  
  
  </div>
</template>





<script>
import Person from "./components/Person.vue";

export default {
 el: '#app',
    components: {
    'person': Person
  },

  data() {
    
    return{
    
   

    new_person: {
      nom: '',
      email: ''
    },
    
    new_black_list:{
        nom1_black_list:'',
        nom2_black_list: ''
    },

    list_afficher : [
     
    ],

    black_list:[


    ],
      
    list:[ 
      
    ],

    list_santa:[

    ]
    }
  },
  

  methods: {
        
    add_person(){
   

      if(this.new_person.nom != ''){   
          

        this.list_afficher.push({
          nom: this.new_person.nom,
          email: this.new_person.email
          
        });
        this.list.push({
          nom_list: this.new_person.nom,
          email_list: this.new_person.email
          
        });
        

        this.new_person.nom='';
        this.new_person.email='';
      }
      
    },

    delete_person(id){
      this.list_afficher.splice(id,1);
      this.list.splice(id,1);
      
    }, 

  


    add_black_list(){

      if(this.new_black_list.nom1_black_list==this.new_black_list.nom2_black_list){
        alert(' Vous ne pouvez pas ajouter la meme personne dans la paire de la liste noire');

         }else if((this.new_black_list.nom1_black_list=='')||(this.new_black_list.nom2_black_list=='')){
            alert('Vous avez choisi qu\'une seule personne !');
            }else{
             this.black_list.push({
                nom1_black_list: this.new_black_list.nom1_black_list,
                nom2_black_list: this.new_black_list.nom2_black_list,
        
             });
             this.new_black_list.nom1_black_list='';
             this.new_black_list.nom2_black_list='';
    
           }
    },

    delete_black_list(id){
      this.black_list.splice(id,1); 
      
    },
    

  tirage_liste(){

  

        if(this.list.length>0){
     
     this.list.sort((a,b) => Math.random() - 0.5); 
       
       
    
      let l = this.list.length-1;
      let b = false;
      while(b==false){
        b = true;

       this.list.sort((a,b) => Math.random() - 0.5);

          if(this.black_list.length>0){

                for (let i = 0; i < this.list.length-1; i++) {
                   for (let j = 0; j < this.black_list.length; j++) {

                       if((this.list[i].nom_list==this.black_list[j].nom1_black_list)&&(this.list[i+1].nom_list==this.black_list[j].nom2_black_list)){
                                b =false;
                      }
                       if((this.list[l].nom_list==this.black_list[j].nom1_black_list)&&(this.list[0].nom_list==this.black_list[j].nom2_black_list)){

                                 b =false;
                          console.log(''+b);
                       }
             }
          }
        }
      }


      for (let i = 0; i < this.list.length-1; i++) {

        this.list_santa.push({
         
          nom1: this.list[i].nom_list,
          email1: this.list[i].email_list,
          nom2: this.list[i+1].nom_list,
          email2: this.list[i+1].email_list,
      
     })
      }

      this.list_santa.push({
         
        nom1: this.list[l].nom_list,
        email1: this.list[l].email_list,
        nom2: this.list[0].nom_list,
        email2: this.list[0].email_list,
    
   })

          
     
    }
   }
    



  },
};


</script>


