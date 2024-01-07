<script>
import axios from "axios";
import UserRow from "./UserRow.vue";
import CreateUser from "./CreateUser.vue";
import TableHeader from "./TableHeader.vue";

export default {
  components: { UserRow, CreateUser, TableHeader },
  data() {
    return {
      loading: true,
      users: [],
      creatingUser: false,
    };
  },
  emits: ["createButton", "editEvent"],
  watch: {
    users(newUsers) {
      this.users = newUsers;
    },
  },
  async mounted() {
    try {
      const res = await axios.get("https://jsonplaceholder.typicode.com/users");
      this.users = res.data;
      this.loading = false;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    create() {
      this.$emit("createButton");
    },
    edit(id) {
      this.$emit("editEvent", id);
    },
    async remove(id) {
      try {
        const res = await axios.delete(
          `https://jsonplaceholder.typicode.com/users/${id}`
        );
        console.log(res.data);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<template>
  <div v-if="loading">Loading...</div>
  <div v-else class="table">
    <div id="buttonContainer">
      <button @click="create" class="createButton">
        <v-icon size="large" color="white" icon="mdi-plus"></v-icon>Create New
        User
      </button>
    </div>
    <TableHeader />
    <UserRow
      v-for="user in users"
      :key="user.id"
      :id="user.id"
      :name="user.name"
      :email="user.email"
      :phone="user.phone"
      @editEvent="edit"
      @deleteEvent="remove"
    ></UserRow>
  </div>
</template>

<style scoped>
.table {
  width: 1220px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.createButton {
  background-color: #3c81fc;
  height: 35px;
  width: auto;
  padding: 10px;
  border-radius: 5px;
  font-family: "Roboto", sans-serif;
  font-weight: 500;
  font-size: 12px;
  color: white;
  align-items: center;
}
#buttonContainer {
  display: flex;
  justify-content: end;
  padding: 5px;
}
</style>
