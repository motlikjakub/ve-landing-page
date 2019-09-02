<template>
  <div class="icon-scroll-wrapper">
    <div class='icon-scroll' v-on:click="scroll('#address')"></div>
  </div>
</template>

<script>
var VueScrollTo = require('vue-scrollto')

export default {
  name: 'ScrollDown',
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  .icon-scroll-wrapper {
    position: absolute;
    left: 0;
    bottom: 10px;
    width: 100%;
    height: 40px;
    z-index: 2;
    text-align: center;
  }

  .icon-scroll,
  .icon-scroll:before {
    position: absolute;
    left: 50%;
  }

  .icon-scroll {
    width: 22px;
    height: 40px;
    margin-left: -11px;
    top: 50%;
    margin-top: -35px;
    border: 2px solid #fff;
    border-radius: 11px;
    box-sizing: border-box;
    cursor: pointer;
  }

  .icon-scroll:before {
    content: '';
    width: 2px;
    height: 8px;
    background: #fff;
    margin-left: -1px;
    top: 6px;
    border-radius: 4px;
    animation-duration: 1.5s;
    animation-iteration-count: infinite;
    animation-name: scroll;
  }

  @keyframes scroll {
    0% {
      opacity: 0;
    }
    15% {
      opacity: 1;
      transform: translateY(0);
    }
    50% {
      opacity: 1;
    }
    100% {
      opacity: 0;
      transform: translateY(15px);
    }
  }

</style>
