<script>
export default {
  emits: ["close"],
  methods: {
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="modal">
      <form action="">
        <div class="grid-item">
          <label for="fname">Full Name</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="address">Address</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="username">Username</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="city">City</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="email">Email</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="zip">Zip Code</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <label for="phone">Phone Nr</label>
          <input type="text" />
        </div>
        <div class="grid-item">
          <div class="coord">
            <div class="sub-item">
              <label for="lat">Latitude</label>
              <input type="text" />
            </div>
            <div class="sub-item">
              <label for="lon">Longitude</label> <input type="text" />
            </div>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<style>
form {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 60px;
  gap: 20px;
}
.coord {
  display: grid;
  grid-template-columns: subgrid;
  gap: 20px;
  grid-template-rows: min-content 1fr;
}
.grid-item {
  grid-column: span 2;
}
.sub-item {
  grid-column: span 1;
}
input {
  border: solid;
}

.modal {
  width: auto;
  padding: 20px;
  margin: auto;
  background: white;
  border-radius: 5px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
</style>
