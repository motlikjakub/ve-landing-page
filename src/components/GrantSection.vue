<template>
  <section class="grant-section" v-bind:class="{ 'not-filled': overlay }">
    <AlertBar v-bind:show="alert_bar_shown" :message="alert_message"/>
    <div class="grant-section-overlay" v-bind:class="{ 'is-active': overlay }">
      <div v-if="grant_address">
        <div class="container">
          <div class="grant-heading is-white">Probíhá výpočet dat...</div>
          <div class="text-center"><div class="loader"></div></div>
        </div>
      </div>
      <div v-else>
        <div class="container">
          <div class="grant-heading is-white">Pro výpočet potřebujeme znát vaši adresu</div>
          <div class="text-center"><Button link="#address" text="Zadat adresu"/></div>
        </div>
      </div>
    </div>
    <div class="grant-section-container container" v-bind:class="{ 'is-active': overlay }">
      <h2 class="grant-heading">Úspory a dotace přímo pro Váš dům</h2>
      <div class="grant-address-map" id="grant"> <!-- Offseted on clients wish -->
        <div class="grant-address-map__half grant-address">
          <div class="grant-address-heading">
            Vaše adresa
          </div>
          <div class="grant-address-content" id="grant-address" v-html="breaked_address"></div>
          <div>
            <a href="#address" class="grant-address-link" v-on:click="scroll('#address')">Zadat novou adresu</a>
          </div>
        </div>
        <div class="grant-address-map__half grant-map" id="mapa">

        </div>
      </div>
      <ProductionChart :productionData=productionData />
      <div class="grant-details">
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.5.</h1>
            <p class="grant-detail__description">6 panelů, Lithiová baterie 2,25 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content">75.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content">
              <animated-number
                :value="smallestGrantProduction"
                :formatValue="formatNumber"
                :duration="300"
              /> kWh
            </p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content">
              <animated-number
                :value="smallestGrantEcoSavings"
                :formatValue="formatNumber"
                :duration="300"
              /> kg</p>
          </div>
        </article>
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.6.</h1>
            <p class="grant-detail__description">10 panelů, Lithiová baterie 3,50 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content darker">105.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content darker">
              <animated-number
                :value="mediumGrantProduction"
                :formatValue="formatNumber"
                :duration="300"
              /> kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content darker">
              <animated-number
                :value="mediumGrantEcoSavings"
                :formatValue="formatNumber"
                :duration="300"
              /> kg</p>
          </div>
        </article>
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.7.</h1>
            <p class="grant-detail__description">14 panelů, Lithiová baterie 5,72 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content darkest">155.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content darkest">
              <animated-number
                :value="largestGrantProduction"
                :formatValue="formatNumber"
                :duration="300"
              /> kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content darkest">
              <animated-number
                :value="largestGrantEcoSavings"
                :formatValue="formatNumber"
                :duration="500"
              /> kg</p>
          </div>
        </article>
      </div>
      <div class="grant-detail__button">
        <Button link="#request-grant" text="Zjistit nárok na dotaci"/>
      </div>
    </div>
  </section>
</template>

<script>
import Button from './Button'
import axios from 'axios'
import AnimatedNumber from 'animated-number-vue'
import AlertBar from './AlertBar'
import { APIendpoint } from '../variables.js'
import ProductionChart from './ProductionChart'
let VueScrollTo = require('vue-scrollto')

export default {
  name: 'GrantSection',
  components: {
    AlertBar,
    Button,
    AnimatedNumber,
    ProductionChart
  },
  data () {
    return {
      smallestGrantProduction: 0,
      smallestGrantEcoSavings: 0,
      mediumGrantProduction: 0,
      mediumGrantEcoSavings: 0,
      largestGrantProduction: 0,
      largestGrantEcoSavings: 0,
      overlay: true,
      alert_bar_shown: false,
      alert_message: 'Pozor',
      productionData: []
    }
  },
  props: {
    grant_address: String
  },
  computed: {
    breaked_address: function () {
      return this.grant_address.replace(',', ', <br/>')
    }
  },
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
    },
    formatNumber: function (number) {
      return Math.round(number).toLocaleString('it-IT')
    },
    handleGrantData: function (address) {
      let selfThis = this
      selfThis.overlay = true

      document.getElementById('mapa').innerHTML = ''
      var coords

      new SMap.Geocoder(address, odpoved)

      function odpoved (geocoder) { /* Odpověď */
        if (!geocoder.getResults()[0].results.length) {
          selfThis.alert_message = 'Tuto adresu bohužel neznáme, pro více informací nás kontaktujte'
          selfThis.alert_bar_shown = true
          return
        }

        var vysledky = geocoder.getResults()[0].results
        var vysledek = vysledky[0]
        loadGrantData(vysledek.coords)

        var stred = SMap.Coords.fromWGS84(vysledek.coords.x, vysledek.coords.y)
        var mapa = new SMap(JAK.gel('mapa'), stred, 19)
        mapa.addDefaultLayer(SMap.DEF_SMART_OPHOTO).enable()
        mapa.addDefaultControls()

        var layer = new SMap.Layer.Marker()
        mapa.addLayer(layer)
        layer.enable()

        var markerIcon = require('../assets/marker.png')
        var options = { url: markerIcon }
        var marker = new SMap.Marker(stred, address, options)
        layer.addMarker(marker)
      }

      function loadGrantData (coords) {
        const formData = new FormData()
        formData.append('latitude', coords.y)
        formData.append('longitude', coords.x)

        axios.post(APIendpoint + '/api/pvgis/vase-elektrarna', formData)
          .then(response => {
            fillData(response.data)
          })
          .catch(error => {
            if (process.env.NODE_ENV === 'development') { // Only if development
              console.log(error)
            }
            selfThis.alert_message = 'Při získávání dat ze serveru nastala chyba, zkuste to znovu později'
            selfThis.alert_bar_shown = true
          })
      }

      function fillData (data) {
        selfThis.smallestGrantProduction = data[0]['production']
        selfThis.smallestGrantEcoSavings = data[0]['eco_savings']
        selfThis.mediumGrantProduction = data[1]['production']
        selfThis.mediumGrantEcoSavings = data[1]['eco_savings']
        selfThis.largestGrantProduction = data[2]['production']
        selfThis.largestGrantEcoSavings = data[2]['eco_savings']
        selfThis.productionData = [
          data[0]['electricity_production']['monthly_productions'],
          data[1]['electricity_production']['monthly_productions'],
          data[2]['electricity_production']['monthly_productions']
        ]
        selfThis.overlay = false
      }
    }
  },
  watch: {
    // When component gets grant_address do the magic
    grant_address: function () {
      this.handleGrantData(this.grant_address)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global";

  .grant-section {
    position: relative;
    width: 100%;
    min-height: 100vh;
    background: #fff;
    padding-top: 55px;
    padding-bottom: 50px;
    box-sizing: border-box;
    transition: .4s;

    @media only screen and (min-width: $screen-md) {
      padding-top: 70px;
      padding-bottom: 70px;
    }

    @media only screen and (min-width: $screen-lg) {
      padding-top: 120px;
      padding-bottom: 100px;
	  }

    &.not-filled {
      max-height: 100vh;
      overflow-y: hidden;

      @media only screen and (min-width: $screen-md) {
        max-height: 100%;
      }
    }
  }

  .grant-section-overlay {
    z-index: 10;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.8);
    color: #fff;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 40px 0;
    box-sizing: border-box;
    pointer-events: none;
    opacity: 0;
    transition: .4s;

    &.is-active {
      opacity: 1;
      pointer-events: all;
    }

    @media only screen and (min-width: $screen-md) {
      align-items: center;
    }
  }

  .loader {
    border: 10px solid rgba(255,255,255,.8); /* Light grey */
    border-top: 10px solid $greenish; /* Greenish */
    border-radius: 50%;
    width: 60px;
    height: 60px;
    animation: spin 1.2s linear infinite;
    margin: 0 auto;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .grant-section-container {
    transition: .4s;

    &.is-active {
      filter: blur(3px);
    }
  }

  .grant-heading {
    color: $rich-black;
    font-family: $nexa;
    font-size: 32px;
    font-weight: 300;
    line-height: 42px;
    letter-spacing: 0.8px;
    text-align: center;
    margin-top: 0;
    margin-bottom: 55px;

    &.is-white {
      color: #fff;
    }

    @media only screen and (min-width: $screen-md) {
      font-size: 40px;
      line-height: 54px;
    }
  }

  .grant-address-map {
    background: #dfe5ec;
    border-radius: 4px;
    max-width: 1130px;
    margin: 0 auto;
    margin-bottom: 55px;
    display: flex;
    flex-wrap: wrap;

    &__half {
      width: 100%;
      text-align: center;

      @media only screen and (min-width: $screen-md) {
        width: 50%;
        text-align: left;
      }
    }
  }

  .grant-address {
    padding: 40px 50px;
    padding-bottom: 35px;
    box-sizing: border-box;
  }

  .grant-address-heading {
    color: $rich-black;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 0.8px;
    line-height: 1;
    margin: 0;
    margin-bottom: 25px;
  }

  .grant-address-content {
    color: $rich-black;
    font-family: $nexa;
    font-size: 20px;
    font-weight: 300;
    letter-spacing: 0.4px;
    line-height: 32px;
    margin-bottom: 35px;
  }

  .grant-address-link {
    color: $greenish;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 1;
    text-decoration: underline;
    letter-spacing: 0.56px;

    &:hover {
      text-decoration: none;
    }
  }

  .grant-map {
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
    min-height: 250px;

    @media only screen and (min-width: $screen-md) {
      min-height: 210px;
      border-top-right-radius: 4px;
      border-bottom-right-radius: 4px;
    }
  }

  .grant-details {
    display: flex;
    flex-wrap: wrap;
    max-width: 1130px;
    margin: 0 auto;
  }

  .grant-detail {
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 20px;

    @media only screen and (min-width: $screen-md) {
      width: 33.33%;
      padding: 0 10px;
      margin-bottom: 0;
    }
  }

  .grant-detail__nzu {
    display: none;

    @media only screen and (min-width: $screen-md) {
      display: initial;
      width: 40px;
      height: auto;
      margin-bottom: 20px;
    }
  }

  .grant-detail__row {
    margin-bottom: 15px;

    @media only screen and (min-width: $screen-md) {
      margin-bottom: 35px;
    }
  }

  .grant-detail__heading {
    color: $rich-black;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 0.8px;
    line-height: 1;
    margin: 0;
    margin-bottom: 5px;

    @media only screen and (min-width: $screen-md) {
      margin-bottom: 10px;
    }
  }

  .grant-detail__description {
    margin: 0;
    font-family: $nexa;
  }

  .grant-detail__content {
    margin: 0;
    margin-top: 5px;
    color: $greenish;
    font-family: $nexa;
    font-size: 28px;
    font-weight: 700;
    letter-spacing: 0.56px;
    line-height: 40px;

    &.darker {
      color: $greenish-darker;
    }

    &.darkest {
      color: $greenish-darkest;
    }

    @media only screen and (min-width: $screen-md) {
      margin-top: 10px;
    }
  }

  .grant-detail__button {
    margin-top: 5px;
    text-align: center;

    @media only screen and (min-width: $screen-md) {
      margin-top: 35px;
    }
  }

</style>
