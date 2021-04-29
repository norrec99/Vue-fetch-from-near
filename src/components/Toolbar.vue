<template>
  <div class="navbar">
    <p>Info</p>
    <h1>CrypTourist</h1>
    <p>Login</p>
  </div>
</template>

<script>
export default {
  name: "Toolbar",
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.navbar {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-color: white;
  padding: 0 4rem;
  padding-top: 20px;
}

p {
  font-size: 1.1rem;
}
</style>