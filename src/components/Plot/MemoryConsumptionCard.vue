<template>
  <div class="card">
    <div id="myDiv"></div>
  </div>
</template>
<script>
  import Card from '../Cards/Card.vue'

  export default {
    name: 'memory-consumption-card',
    components: {
      Card
    },
    props: {
      chartType: {
        type: String,
        default: 'Line' // Line | Pie | Bar
      },
      chartOptions: {
        type: Object,
        default: () => {
          return {}
        }
      },
      chartData: {
        type: Object,
        default: () => {
          return {
            labels: [],
            series: []
          }
        }
      },
      responsiveOptions: [Object, Array]
    },
    data () {
      return {
        trace1: null,
        trace2: null,
        chartId: 'no-id',
        $Chartist: null,
        chart: null
      }
    },
    methods: {
      updatePlot () {
        Plotly.d3.csv("https://raw.githubusercontent.com/plotly/datasets/master/finance-charts-apple.csv", function(err, rows) {
          function unpack(rows, key) {
            return rows.map(function(row) {
              return row[key];
            });
          }

          var trace1 = {
            type: "scatter",
            mode: "lines",
            x: unpack(rows, 'Date'),
            y: unpack(rows, 'AAPL.High'),
            line: {color: '#17BECF'}
          }

          var trace2 = {
            type: "scatter",
            mode: "lines",
            x: unpack(rows, 'Date'),
            y: unpack(rows, 'AAPL.Low'),
            line: {color: '#7F7F7F'}
          }

          var data = [trace1,trace2];

          var layout = {
            title: 'Memory Consumption',
            xaxis: {
              range: ['2016-07-01', '2016-12-31'],
              type: 'date'
            },
            yaxis: {
              autorange: true,
              range: [86.8700008333, 138.870004167],
              type: 'linear'
            }
          };

          Plotly.newPlot('myDiv', data, layout);
        })
      }
    },
    async mounted () {
      this.updatePlot()
    }
  }
</script>
<style>

</style>
