<template>
  <form enctype="multipart/form-data">
    <p>Upload a CSV file</p>
    <input type="file" @change="onFileChange" />
  </form>
</template>

<script>
//import csv from "csvtojson";
export default {
  name: "CsvUpload",
  data() {
    return {
      items: [],
    };
  },
  mounted() {},
  methods: {
    onFileChange(e) {
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.createInput(files[0]);
      // const csvFilePath = 'person.csv';
      // (async () => {
      //   const jsonObj = await csv().fromFile(csvFilePath)
      //   console.log(jsonObj);
      // })();
    },
    createInput(file) {
      let promise = new Promise((resolve) => {
        var reader = new FileReader();
        reader.onload = () => {
          resolve(reader.result);
        };
        reader.readAsText(file);
      });
      promise.then(
        (data) => {
          /* handle a successful result */
          console.log(data);
          let cells = data.split("\n").map(function (el) {
            return el.split(",");
          });
          let headings = cells.shift();
          this.items = cells.map(function (el) {
            let obj = {};
            for (let i = 0, l = el.length; i < l; i++) {
              obj[headings[i]] = isNaN(Number(el[i])) ? el[i] : +el[i];
            }
            return obj;
          });
          this.$emit("tableData", this.items);
          //console.log(this.CSVtoArray(this.fileinput))
        },
        (error) => {
          /* handle an error */
          console.log(error);
        }
      );
    },
    CSVtoArray(text) {
      // https://stackoverflow.com/questions/8493195/how-can-i-parse-a-csv-string-with-javascript-which-contains-comma-in-data
      var re_valid = /^\s*(?:'[^'\\]*(?:\\[\S\s][^'\\]*)*'|"[^"\\]*(?:\\[\S\s][^"\\]*)*"|[^,'"\s\\]*(?:\s+[^,'"\s\\]+)*)\s*(?:,\s*(?:'[^'\\]*(?:\\[\S\s][^'\\]*)*'|"[^"\\]*(?:\\[\S\s][^"\\]*)*"|[^,'"\s\\]*(?:\s+[^,'"\s\\]+)*)\s*)*$/;
      var re_value = /(?!\s*$)\s*(?:'([^'\\]*(?:\\[\S\s][^'\\]*)*)'|"([^"\\]*(?:\\[\S\s][^"\\]*)*)"|([^,'"\s\\]*(?:\s+[^,'"\s\\]+)*))\s*(?:,|$)/g;

      // Return NULL if input string is not well formed CSV string.
      if (!re_valid.test(text)) return null;

      var a = []; // Initialize array to receive values.
      text.replace(
        re_value, // "Walk" the string using replace with callback.
        function (m0, m1, m2, m3) {
          // Remove backslash from \' in single quoted values.
          if (m1 !== undefined) a.push(m1.replace(/\\'/g, "'"));
          // Remove backslash from \" in double quoted values.
          else if (m2 !== undefined) a.push(m2.replace(/\\"/g, '"'));
          else if (m3 !== undefined) a.push(m3);
          return ""; // Return empty string.
        }
      );

      // Handle special case of empty last value.
      if (/,\s*$/.test(text)) a.push("");
      return a;
    },
  },
};
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
