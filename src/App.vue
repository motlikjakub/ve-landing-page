<template>
  <div id="app">
    <img class="logo" alt="Vaše elektrárna logo" src="./assets/VE-logo.svg">
    <HeaderSwiper/>
    <AddressSection/>
    <GrantSection/>
    <GetGrant/>
    <Footer/>
  </div>
</template>

<script>
import HeaderSwiper from './components/HeaderSwiper'
import AddressSection from './components/AddressSection'
import GrantSection from './components/GrantSection'
import GetGrant from './components/GetGrantSection'
import Footer from './components/Footer'

export default {
  name: 'app',
  components: {
    AddressSection,
    HeaderSwiper,
    GrantSection,
    GetGrant,
    Footer
  }
}
</script>

<style lang="scss">
@import "~normalize.css/normalize.css";
@import "./scss/variables.scss";

.logo {
  position: absolute;
  z-index: 10;
  width: 66px;
  height: 33px;
  top: 31px;
  left: 50%;
  margin-left: -33px;

  @media only screen and (min-width: $screen-md) {
    width: 93px;
    height: 48px;
    top: 54px;
	}

  @media only screen and (min-width: $screen-lg) {
    width: 122px;
    height: 59px;
    top: 64px;
	}
}

#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  // -webkit-font-smoothing: antialiased;
  // -moz-osx-font-smoothing: grayscale;
  position: relative;
}
</style>
