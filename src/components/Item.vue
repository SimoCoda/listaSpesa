<template>
  <div>
    <h1>{{ fullItemName }}</h1>
    <button @click="deleteItem">Elimina elemento</button>
    <button @click="updateItem">Aggiorna</button>

    <div v-if="isModalOpen">
      <input type="text" v-model="updatedItemName" />
      <button @click="confirmUpdateName">Conferma</button>
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
      updatedItemName: "",
    };
  },
  methods: {
    deleteItem() {
      this.$emit("delete-item", this.id);
    },
    updateItem() {
      this.isModalOpen = true;
    },
    confirmUpdateName() {
        console.log('updating...')
        this.$emit("update-name", { id: this.id, newName: this.updatedItemName });
        this.isModalOpen = false
        this.updatedItemName = ''
    },
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
