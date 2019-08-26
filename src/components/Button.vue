<template>

  <a :href="link" class="greenish-button greenish-button--smaller" v-on:click="scroll(link)" v-if="smaller">
    {{text}}
  </a>
  <a :href="link" class="greenish-button" v-on:click="scroll(link)" v-else>
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
    smaller: Boolean
  },
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
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
