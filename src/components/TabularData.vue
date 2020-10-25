<template>
  <div class="ml-2">
    <b-table striped hover :items="items"></b-table>
  </div>
</template>


<script>
import { BTable } from "bootstrap-vue";
export default {
  name: 'TabularData',
  props: ['items'],
  components: {
      BTable
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div {
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
