<template>
  <div class="card">
    <div id="myDiv"></div>
  </div>
</template>
<script>
import Card from '../Cards/Card.vue'
import axios from "axios";

export default {
  name: 'memory-consumption-card',
  components: {
    Card
  },
  async mounted() {
    axios
        .get('http://localhost:8080/api/plots/memory')
        .then(function (response) {
          Plotly.newPlot('myDiv', response.data.data, response.data.layout);
        })
  }
}
</script>
