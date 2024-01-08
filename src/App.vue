<script>
import CreateUser from "./components/CreateUser.vue";
import EditUser from "./components/EditUser.vue";
import UsersTable from "./components/UsersTable.vue";

export default {
  components: {
    UsersTable,
    CreateUser,
    EditUser,
  },

  data() {
    return {
      creatingUser: false,
      editingUser: false,
      id: "",
    };
  },

  methods: {
    toggleModal() {
      this.creatingUser = !this.creatingUser;
    },

    toggleEdit(id) {
      this.id = id;
      this.editingUser = !this.editingUser;
    },
  },
};
</script>

<template>
  <UsersTable @createButton="toggleModal" @editEvent="toggleEdit" />

  <div v-if="creatingUser">
    <CreateUser @close="toggleModal" />
  </div>

  <div v-if="editingUser">
    <EditUser @close="toggleEdit" :id="this.id" />
  </div>
</template>
