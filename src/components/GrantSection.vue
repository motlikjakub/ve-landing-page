<template>
  <section class="grant-section" id="grant">
    <div class="grant-section-container container">
      <h2 class="grant-heading">Vaše lokace, kde můžete získat dotaci</h2>
      <div class="grant-address-map">
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
      <div class="grant-details">
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.4.</h1>
            <p class="grant-detail__description">6 panelů, Lithiová baterie 2,25 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content">75.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content">1.750 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content">2.143 kg</p>
          </div>
        </article>
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.5.</h1>
            <p class="grant-detail__description">10 panelů, Lithiová baterie 3,50 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content">105.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content">3.230 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content">3.792 kg</p>
          </div>
        </article>
        <article class="grant-detail">
          <div>
            <img class="grant-detail__nzu" src="../assets/nzu.png" alt="Nová Zelená Úsporám">
          </div>
          <div class="grant-detail__row">
            <h1 class="grant-detail__heading">Dotační titul NZÚ C.3.4.</h1>
            <p class="grant-detail__description">14 panelů, Lithiová baterie 5,72 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Dotace</h2>
            <p class="grant-detail__content">155.000 Kč</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Roční výroba a úspora</h2>
            <p class="grant-detail__content">4.850 kWh</p>
          </div>
          <div class="grant-detail__row">
            <h2 class="grant-detail__heading">Úspora CO<sub>2</sub></h2>
            <p class="grant-detail__content">5.675 kWh</p>
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
import Button from './Button';
import axios from 'axios';
let VueScrollTo = require('vue-scrollto');

export default {
  name: 'GrantSection',
  components: {
    Button
  },
  props: {
    grant_address: String
  },
  computed: {
    breaked_address: function() {
      return this.grant_address.replace(',', ', <br/>');
    }
  },
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
    },
    handleGrantData: function (address) {
      document.getElementById('mapa').innerHTML = '';
      var coords;

      new SMap.Geocoder(address, odpoved);

      function odpoved(geocoder) { /* Odpověď */
          if (!geocoder.getResults()[0].results.length) {
              alert("Tuto adresu bohužel neznáme, pro více informací nás prosím kontaktujte nás.");
              return;
          }
          
          var vysledky = geocoder.getResults()[0].results;
          var vysledek = vysledky[0];
          loadGrantData(vysledek.coords);

          var stred = SMap.Coords.fromWGS84(vysledek.coords.x, vysledek.coords.y);
          var mapa = new SMap(JAK.gel("mapa"), stred, 16);
          mapa.addDefaultLayer(SMap.DEF_BASE).enable();
          mapa.addDefaultControls();

          var layer = new SMap.Layer.Marker();
          mapa.addLayer(layer);
          layer.enable();

          var markerIcon = require('../assets/marker.png')
          var options = {url: markerIcon};
          var marker = new SMap.Marker(stred, address, options);
          layer.addMarker(marker);
      }

      function loadGrantData (coords) {
        console.log(coords);
        axios.post('https://vaseelektrarna.adwell.cz/api/pvgis/vase-elektrarna',{ 
          params: { 
            latitude: coords.x,
            longtitude: coords.y
          }
        })
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.log(e);
        });
      }
    },
  },
  watch: {
    // When component gets grant_address do the magic
    grant_address: function () {
      this.handleGrantData(this.grant_address);
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

    @media only screen and (min-width: $screen-md) {
      padding-top: 70px;
      padding-bottom: 70px;
    }
    
    @media only screen and (min-width: $screen-lg) {
      padding-top: 120px;
      padding-bottom: 100px;
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
    margin-bottom: 85px;
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
