<template>
  <section class="header-swiper">
    <!-- Slider main container -->
    <div class="swiper-container">
      <!-- Additional required wrapper -->
      <div class="swiper-wrapper">
        <!-- Slides -->
        <div class="swiper-slide slide-1">
          <div class="swiper-slide-bcg"></div>
          <div class="swiper-slide-inner">
            <div class="swiper-slide-text">
              Chraňte naši Zemi, <br>
              ať se nám lépe dýchá.
            </div>
            <div class="container">
              <Button link="#address" text="Chci spočítat vlastní výrobu"/>
            </div>
          </div>
        </div>
        <div class="swiper-slide slide-2">
          <div class="swiper-slide-bcg"></div>
          <div class="swiper-slide-inner">
            <div class="swiper-slide-text">
              Budoucnost našich dětí, <br>
              je v našich rukou.
          </div>
          <div class="container">
            <Button link="#address" text="Chci spočítat vlastní výrobu"/>
          </div>
          </div>
        </div>
        <div class="swiper-slide slide-3">
          <div class="swiper-slide-bcg"></div>
          <div class="swiper-slide-inner">
            <div class="swiper-slide-text">
              Buďte nezávislí <br>
              a svobodní.
          </div>
          <div class="container">
            <Button link="#address" text="Chci spočítat vlastní výrobu"/>
          </div>
          </div>
        </div>
        <div class="swiper-slide slide-4">
          <div class="swiper-slide-bcg"></div>
          <div class="swiper-slide-inner">
            <div class="swiper-slide-text">
              Vyrobte si svoji vlastní <br>
              elektrickou energii.
          </div>
          <div class="container">
            <Button link="#address" text="Chci spočítat vlastní výrobu"/>
          </div>
          </div>
        </div>
      </div>
      <!-- If we need pagination -->
      <div class="swiper-pagination"></div>

      <!-- If we need scrollbar -->
      <div class="swiper-scrollbar"></div>
    </div>
    <ScrollDown/>
  </section>
</template>

<script>
import Swiper from 'swiper/dist/js/swiper.esm.bundle'
import Button from './Button'
import ScrollDown from './ScrollDown'

export default {
  name: 'HeaderSwiper',
  components: {
    ScrollDown,
    Button
  },
  data () {
    return {
    }
  },
  mounted () {
    const self = this
    const swiper = new Swiper('.swiper-container', {
      speed: 1000,
      autoplay: {
        delay: 5000
      },
      pagination: {
        el: '.swiper-pagination',
        clickable: true
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "~swiper/src/swiper";
  @import "../scss/global";

  $slide-zoom-speed: 10s;
  $slide-zoom-size: 1.15;

  @mixin slide-background {
    background-color: #ccc;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
  }

  @mixin slide-darken {
    position: relative;

    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
      background: rgba(0, 0, 0, .4);
    }
  }

  .header-swiper {
    position: relative;
  }

  .swiper-slide-active {
    .swiper-slide-bcg {
      transform: scale($slide-zoom-size);
    }
  }

  .swiper-slide {
    position: relative;
    min-height: 100vh;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;

    &.slide-1 {
      .swiper-slide-bcg {
        background: url("../assets/slides/ve1.jpg");
        @include slide-background;
      }
    }

    &.slide-2 {
      .swiper-slide-bcg {
        background: url("../assets/slides/ve2.jpg");
        @include slide-background;
      }
    }

    &.slide-3 {
      .swiper-slide-bcg {
        background: url("../assets/slides/ve3.jpg");
        @include slide-background;
      }
    }

    &.slide-4 {
      .swiper-slide-bcg {
        background: url("../assets/slides/ve4.jpg");
        @include slide-background;
      }
    }
  }

  .swiper-slide-bcg {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    transition: $slide-zoom-speed linear;
  }

  .swiper-slide-inner {
    position: relative;
    z-index: 2;
    text-align: center;
  }

  .swiper-slide-text {
    color: #fff;
    font-family: $nexa;
    font-size: 45px;
    font-weight: 300;
    line-height: 1.15;
    letter-spacing: 1.8px;
    text-align: center;
    margin-bottom: 85px;
    padding: 0 20px;

    @media only screen and (min-width: $screen-md) {
      padding: 0 40px;
      font-size: 60px;
      margin-bottom: 85px;
	  }

    @media only screen and (min-width: $screen-lg) {
      font-size: 90px;
      margin-bottom: 110px;
	  }
  }

  .swiper-slide-button {
    margin: 0 auto;
  }

  .swiper-pagination {
    display: none;

    @media only screen and (min-width: $screen-md) {
      display: block;
      position: absolute;
      z-index: 3;
      right: 0;
      top: 50%;
      margin-top: -50px;
	  }
  }

</style>

<style lang="scss">
  @import "../scss/variables";

  .swiper-pagination-bullet {
    display: block;
    width: 16px;
    height: 3px;
    background: #fff;
    margin-bottom: 25px;
    margin-right: 19px;
    transition: 1s;
    cursor: pointer;
  }

  .swiper-pagination-bullet-active {
    width: 35px;
    margin-right: 0;
    background: $greenish;
  }

</style>
