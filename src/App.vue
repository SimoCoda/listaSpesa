<template>
  <div id="app">
    <form @submit.prevent="addToList">
      <input type="text" placeholder="Aggiungi alla lista..." v-model="item.nome"/>
      <button type="submit">Aggiungi!</button>
    </form>

    <div v-if="listaSpesaHasElement" class="lista">
      <h1>Lista della spesa: </h1>
      <Item @delete-item="removeItem" @update-item="changeItem" v-for="elementoSpesa in listaSpesa" :key="elementoSpesa.id" :id="elementoSpesa.id" :nome="elementoSpesa.nome"/>
    </div>

    <div v-else>
      <h1>La lista è vuota... aggiungi un elemento per iniziare!</h1>
    </div>
  </div>
</template>

<script>
import Item from './components/Item.vue'
export default {
  name: 'App',
  components:{
    Item
  },

  data: () => {
    return {
      listaSpesa: [],
      item: { id: 0, nome: '' },
      isLoading: false
    }
  },

  mounted(){
    console.log('Scarico i dati.....')
  },
  created(){
    console.log('Application is created!')
  },
  updated(){
    console.log('Aggiornato!') 
  },

  watch: {
    listaSpesa(){
      if(this.listaSpesa.length === 5){
        alert('Hai raggiunto 5 prodotti nella tua lista!')
        // Invio al server la lista
        // Resetto la lista locale
        this.listaSpesa = []
      }
    }
  },

  computed: {
    listaSpesaHasElement(){
      return this.listaSpesa.length > 0;
    }
  },

  methods: {

    toggleIsLoading(){
      console.log(this.isLoading)
      this.isLoading = !this.isLoading
    },

    addToList(){
      const el = {
        id: this.item.id,
        nome: this.item.nome,
      }



      if(el.nome.trim() === ''){
        console.log("Il nome non può essere vuoto!")
        return;
      }
      console.log("Il nome è valorizzato correttamente!")
      
      this.listaSpesa.push(el);

      this.item.nome = '';
      this.item.id++;
    },


    
    removeItem(idItem){
      console.log(idItem);
      this.listaSpesa = this.listaSpesa.filter(el => {
        console.log("Elemento lista spesa: " + el.id + ". Elemento da eliminare: " + idItem)
        return el.id !== idItem
      })
    },

    changeItem({id, newName}){

      console.log('New item name');

      this.listaSpesa = this.listaSpesa.map(el => {
        if(el.id === id){
          return el.nome = newName
        }else{
          return el;
        }
      })
    }
    }

  }

</script>





<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
