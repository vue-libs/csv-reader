<template>
  <div id="app">
    <CsvUpload @tableData="items = $event"></CsvUpload>
    <TabularData :items="items"></TabularData>
  </div>
</template>

<script>
import CsvUpload from "./components/CsvUpload";
import TabularData from "./components/TabularData";
export default {
  name: "App",
  components: {
    CsvUpload,
    TabularData,
  },
  data() {
    return {
      items: [],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@import "~bootstrap/dist/css/bootstrap.css";
</style>
