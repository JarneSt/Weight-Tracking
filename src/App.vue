<template>
  <div id="nav">
    <h1>Weight Tracker</h1>
  </div>
  <router-view/>
</template>

<style scoped>
#nav h1 {
  color: white;
}
</style>
<style>
#app {
  font-family: "Poppins Light",serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  background: #2fb5ee;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;

}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
