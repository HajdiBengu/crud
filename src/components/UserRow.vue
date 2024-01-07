<script>
export default {
  props: ["id", "name", "email", "phone"],
};
</script>

<template>
  <div class="row">
    <div class="id">{{ id }}</div>
    <div class="item">{{ name }}</div>
    <div class="item">{{ email }}</div>
    <div class="item">{{ phone }}</div>
    <div class="buttons">
      <button>
        <v-icon size="large" color="grey-lighten-1" icon="mdi-pencil"></v-icon>
      </button>
      <button>
        <v-icon
          size="large"
          color="grey-lighten-1"
          icon="mdi-trash-can-outline"
        ></v-icon>
      </button>
    </div>
  </div>
</template>

<style scoped>
.row {
  padding: 20px 15px 20px 20px;
  height: 45px;
  margin: 5px;
  border-radius: 5px;
  border: solid 1px #e7eaf3;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.item {
  justify-content: left;
  min-width: 250px;
}
.id {
  min-width: 100px;
}
button {
  margin-left: 10px;
}
div {
  color: #4e5964;
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-size: 12px;
}
</style>
