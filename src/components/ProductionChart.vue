<template>
  <section class="production-chart">
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
  mounted () {
    var ctx = 'productionChart'

    var myChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: ['Leden', 'Únor', 'Březen', 'Duben', 'Květen', 'Červen', 'Červenec', 'Srpen', 'Září', 'Říjen', 'Listopad', 'Prosinec'],
        datasets: [{
          label: 'Dotační titul NZÚ C.3.5. Výkon (kWh)',
          data: [12, 19, 3, 9, 11, 5, 2, 3, 4, 8, 8, 23],
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
          data: [12, 19, 8, 8, 9, 11, 23, 3, 5, 2, 3, 4],
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
          data: [2, 3, 4, 8, 12, 19, 3, 8, 5, 9, 11, 23],
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
