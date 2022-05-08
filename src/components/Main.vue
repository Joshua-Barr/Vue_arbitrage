<template>
  <div class="hello">
    <p></p>
    <OddsAPI/>
    
    <h3>Other factors not considered in the calculated odds:</h3>
    <ul>
      <li>AFL odds of a draw: ~1.01%</li>
      <li>MLB odds of a draw: 0%</li>
    </ul>
    <p></p>
  </div>
</template>

<script>
import OddsAPI from './OddsAPI.vue'

export default {
  components: { OddsAPI },
  name: 'MainPage',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
