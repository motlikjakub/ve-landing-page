<template>
  <section class="production-chart" v-if="productionData">
    <div class="production-chart-mobile-wrapper">
      <canvas id="productionChart" width="400" height="100"></canvas>
    </div>
  </section>
</template>

<script>
import Chart from 'chart.js'

export default {
  name: 'ProductionChart',
  props: {
    productionData: Array
  },
  methods: {
    renderChart: function (data) {
      var ctx = 'productionChart'

      var myChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Leden', 'Únor', 'Březen', 'Duben', 'Květen', 'Červen', 'Červenec', 'Srpen', 'Září', 'Říjen', 'Listopad', 'Prosinec'],
          datasets: [{
            label: 'Dotační titul NZÚ C.3.5. Výkon (kWh)',
            data: [
              data[0][0]['production'],
              data[0][1]['production'],
              data[0][2]['production'],
              data[0][3]['production'],
              data[0][4]['production'],
              data[0][5]['production'],
              data[0][6]['production'],
              data[0][7]['production'],
              data[0][8]['production'],
              data[0][9]['production'],
              data[0][10]['production'],
              data[0][11]['production']
            ],
            backgroundColor: [
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)',
              'rgba(0, 197, 190, 0.7)'
            ],
            borderColor: [
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)',
              'rgba(0, 197, 190, 1)'
            ],
            borderWidth: 1
          },
          {
            label: 'Dotační titul NZÚ C.3.6. Výkon (kWh)',
            data: [
              data[1][0]['production'],
              data[1][1]['production'],
              data[1][2]['production'],
              data[1][3]['production'],
              data[1][4]['production'],
              data[1][5]['production'],
              data[1][6]['production'],
              data[1][7]['production'],
              data[1][8]['production'],
              data[1][9]['production'],
              data[1][10]['production'],
              data[1][11]['production']
            ],
            backgroundColor: [
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)',
              'rgba(0, 171, 165, 0.7)'
            ],
            borderColor: [
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)',
              'rgba(0, 171, 165, 1)'
            ],
            borderWidth: 1
          },
          {
            label: 'Dotační titul NZÚ C.3.7. Výkon (kWh)',
            data: [
              data[2][0]['production'],
              data[2][1]['production'],
              data[2][2]['production'],
              data[2][3]['production'],
              data[2][4]['production'],
              data[2][5]['production'],
              data[2][6]['production'],
              data[2][7]['production'],
              data[2][8]['production'],
              data[2][9]['production'],
              data[2][10]['production'],
              data[2][11]['production']
            ],
            backgroundColor: [
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)',
              'rgba(1, 145, 140, 0.7)'
            ],
            borderColor: [
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)',
              'rgba(1, 145, 140, 1)'
            ],
            borderWidth: 1
          }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    }
  },
  watch: {
    productionData: function () {
      this.renderChart(this.productionData)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global.scss";

  .production-chart {
    max-width: 1130px;
    margin: 0 auto;
    margin-bottom: 70px;
    overflow-x: scroll;

    @media only screen and (min-width: 840px) {
      overflow-x: auto;
    }
  }

  .production-chart-mobile-wrapper {
    min-width: 800px;
  }
</style>
