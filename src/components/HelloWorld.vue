<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    <webpage-shot :visible.sync='visible'></webpage-shot>
    <button @click="visible=true">start webpage shot</button>
  </div>
</template>

<script>
import webpageShot from "@/components/shot";
export default {
  name: 'HelloWorld',
  components:{
    webpageShot
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      visible: false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
