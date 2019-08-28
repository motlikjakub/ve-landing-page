<template>

  <a :href="link" class="greenish-button" :class="classNames" v-on:click="click(link)">
    {{text}}
  </a>
</template>

<script>
var VueScrollTo = require('vue-scrollto');

export default {
  name: 'Button',
  props: {
    link: String,
    text: String,
    classNames: String
  },
  methods: {
    click: function (link) {
      event.preventDefault();
      if (event.target.classList.contains('checkbox-required')) {
        if (!document.querySelector('#address-input-agree:checked')) {
          alert('Je nutné souhlasit se zpracováním osobních údajů');
        } else {
          VueScrollTo.scrollTo(link);
          let address = document.querySelector('#address-input-address').value;
          this.loadmap(address);   
          document.querySelector('#grant-address').innerHTML = address.replace(',', ', <br/>');
          document.querySelector('#get-grant-address').value = address;
        }
      } else {
        VueScrollTo.scrollTo(link);
      }
    },
    loadmap: function (address) {
      new SMap.Geocoder(address, odpoved);

      function odpoved(geocoder) { /* Odpověď */
          if (!geocoder.getResults()[0].results.length) {
              alert("Tuto adresu bohužel neznáme, prosíme kontaktujte nás.");
              return;
          }
          
          var vysledky = geocoder.getResults()[0].results;
          var vysledek = vysledky[0];
          console.log(vysledek);

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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global.scss";

  .greenish-button {
    color: #fff;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    line-height: 1;
    letter-spacing: 0.9px;
    display: inline-block;
    text-align: center;
    padding: 14px 28px;
    box-sizing: border-box;
    background: $greenish;
    border-radius: 4px;
    cursor: pointer;
    transition: .3s;
    width: 100%;
    text-decoration: none;

    @media only screen and (min-width: $screen-md) {
      font-size: 16px;
      padding: 22px 28px;
      width: auto;
	  }

    @media only screen and (min-width: $screen-lg) {
      font-size: 18px;
      padding: 26px 38px;
	  }

    &:hover {
      background: #029e99;
    }

    &--smaller {
      font-size: 14px;
      padding: 14px 28px;
      padding: 19px 27px;

      @media only screen and (min-width: $screen-md) {
        font-size: 16px;
        padding: 19px 27px;
	    }
    }
  }
</style>
