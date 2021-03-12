<template lang="html">

  <div class="person">
    <div>
      <h3 class="title_nom">{{data.nom}}</h3>
      <p v-if="data.email!=''" class="title_email">{{data.email}}</p>
      
    </div>
    <button class="person_added" title="supprimer" @click="person_added()">❌</button>
</div>


</template>

<script>

export default { 

props: ['data'],
  data() {
    return{
      
    }
  },
  methods: {
    person_added(){
      this.$emit('person_added')
    }

  }

}

</script>