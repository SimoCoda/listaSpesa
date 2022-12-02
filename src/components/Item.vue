<template>
  <div>
    <h1>{{ fullItemName }}</h1>
    <button @click="deleteItem(id)">Elimina elemento</button>
    <button @click="updateItem">Aggiorna</button>

    <div v-if="isModalOpen">
      <input v-model="newName" type="text" @keydown.enter="confirmUpdate(id,newName)"/>
      <button @click="confirmUpdate(id,newName)">Conferma</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: {
    id: {
      required: true,
      type: Number,
    },
    nome: {
      required: true,
      type: String,
    },
  },
  data: () => {
    return {
      isModalOpen: false,
      newName: ""
    };
  },
  methods: {
    deleteItem(id) {
      this.$emit("delete-item", id);
    },
    updateItem() {
        this.isModalOpen = true;
    },
    confirmUpdate(id, nome){
        if(nome.trim() === ""){
            return
        }
        this.$emit("update-item", {id , nome})
        this.newName = ""
    }
  },
  computed: {
    fullItemName() {
      return `Oggetto della spesa con ID: ${this.id} e nome: ${this.nome}`;
    },
    itemId() {
      return `Oggetto ID: ${this.id}`;
    },
  },
};
</script>
