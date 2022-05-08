<template>
  <div class="nav">
    <img alt="Arb Logo" src="./assets/Arbitrage-logos_transparent.png" height="180" class="header-image">
  </div>
  <div>
    <MainPage msg="Welcome to Auto-Arbitrage, calculate returns for team sports."/>
  </div>
  <div class="nav">
    <h3>Built by <a href="https://www.linkedin.com/in/joshua-barr-03049a10b/w">Joshua Barr</a> on Vue with a backend API</h3>
    <p></p>
  </div>
</template>

<script>
import MainPage from './components/Main.vue'

export default {
  name: 'App',
  components: {
    MainPage
  }
}
</script>

<style>
#app {
  font-family: monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
    display: block;
    margin: 0px;
}

button {
    height: 25px;
    margin-top: 15px;
    margin-left: 5px;
    margin-right: 5px;
    margin-bottom: 5px;
}

.fill {
  font-size: 15px;
  font-weight: 150;
  letter-spacing: 1px;
  padding: 2px 10px 2px;
  outline: 0;
  border: 1px solid black;
  cursor: pointer;
  position: relative;
  background-color: rgba(0, 0, 0, 0);
}

.fill::after {
  content: "";
  background-color: #F9A825;
  width: 100%;
  z-index: -1;
  position: absolute;
  height: 100%;
  top: 2px;
  left: 4px;
  transition: 0.2s;
}

.fill:hover::after {
  top: 0px;
  left: 0px;
}

.nav {
  background-color: #F9A825;
  overflow: hidden;
}

</style>
