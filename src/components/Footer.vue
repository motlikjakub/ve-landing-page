<template>
  <footer class="footer">
    <div class="container footer-container">
      <div class="footer__half footer_half--left">
        <img class="footer__half__img" src="../assets/nzu.png" alt="nová zelená úsporám">
        <img class="footer__half__img" src="../assets/sfzp.png" alt="Státní fond životního prostředí ČR">
      </div>
      <div class="footer__half footer__half--right">
        <div class="footer__half__row">
          <a href="" class="footer__half__link">Zásady ochrany soukromí</a>
          <a href="" class="footer__half__link">Používání cookies</a>
          <a href="" class="footer__half__link">Podmínky použití</a>
        </div>
        <div class="footer__half__row">
          © Copyright FVE. Všechna práva vyhrazena.
        </div>
      </div>
    </div>
  </footer>
</template>

<script>

export default {
  name: 'Footer'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global";

  .footer {
    background: #fff;
    padding-top: 50px;
    padding-bottom: 40px;

    @media only screen and (min-width: $screen-md) {
      padding-bottom: 90px;
	  }
  }

  .footer-container {
    max-width: 1500px;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }

  .footer__half {
    width: 100%;
    text-align: center;
    margin-bottom: 45px;

    &:nth-of-type(2) {
      margin: 0;
    }

    @media only screen and (min-width: $screen-md) {
      width: 50%;
      margin-bottom: 0;
      text-align: left;
	  }
  }

  .footer__half__img {
    height: 47px;
    width: auto;
    margin-right: 30px;

    &:nth-of-type(2) {
      margin-right: 0;
    }
  }

  .footer__half__row {
    color: #56626c;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 36px;
    letter-spacing: 0.7px;
    text-align: center;
    margin-bottom: 25px;

    &:nth-of-type(2) {
      margin-bottom: 0;
    }

    @media only screen and (min-width: $screen-md) {
      text-align: right;
      margin-bottom: 0;
	  }
  }

  .footer__half__link {
    text-decoration: none;
    color: #56626c;
    display: block;

    @media only screen and (min-width: $screen-md) {
      display: inline-block;
      margin-left: 35px;
	  }

    &:hover {
      text-decoration: underline;
    }
  }

</style>
