<template>
  <div id="app">
    <Calc v-bind:title="message"
      v-on:result-event="appAction"
    />
    <hr>
    <div><table v-html="log"></table></div>
  </div>
</template>

<script>
import Calc from "./components/Calc";

export default {
  name: 'app',
  components: {
    Calc,
  },
  data: function () {
    return {
      message: 'CALC',
      result: [],
    };
  },
  computed: {
    log: function () {
      var table = '<tr><th class="head">Expression</th><th class="head">Value</th></tr>';
      for (var i in this.result) {
        table += '<tr><td>' + this.result[i][0] + '</td><td>' + this.result[i][1] + '</td></tr>';
      }
      return table;
    }
  },
  created: function () {
    var items = localStorage.getItem('log');
    var logs = JSON.parse(items);
    if (logs != null) { this.result = logs; }
  },
  methods: {
    appAction: function (exp, res) {
      this.result.unshift([exp, res]);
      if (this.result.length > 10) {
        this.result.pop();
      }
      var log = JSON.stringify(this.result);
      localStorage.setItem('log', log);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 5px;
}

tr td {
  padding: 5px;
  border: 1px solid gray;
}

tr th {
  padding: 5px;
  border: 1px solid gray;
}

tr th.head {
  background-color: black;
  color: white;
}

</style>
