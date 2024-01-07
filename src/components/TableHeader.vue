<template>
  <div id="container">
    <div id="id">ID</div>
    <div id="item">Name</div>
    <div id="item">Email</div>
    <div id="item">Phone</div>
    <div>Actions</div>
  </div>
</template>

<style scoped>
#container {
  padding: 20px;
  height: 45px;
  margin: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
#item {
  justify-content: left;
  min-width: 250px;
}
#id {
  min-width: 100px;
}
div {
  color: #4e5964;
  font-weight: 500;
  font-family: "Roboto", sans-serif;
  font-size: 14px;
}
</style>
