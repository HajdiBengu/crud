<script>
import CreateUser from "./components/CreateUser.vue";
import UsersTable from "./components/UsersTable.vue";

export default {
  components: {
    UsersTable,
    CreateUser,
  },
  data() {
    return {
      creatingUser: false,
    };
  },
  methods: {
    toggleModal() {
      this.creatingUser = !this.creatingUser;
    },
  },
};
</script>

<template>
  <UsersTable @createButton="toggleModal" />
  <div v-if="creatingUser">
    <CreateUser @close="toggleModal" />
  </div>
</template>
