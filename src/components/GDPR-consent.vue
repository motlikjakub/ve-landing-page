<template>
  <div class="gdpr-consent">
    <div class="gdpr-consent-inner">
      <div class="gdpr-consent-text">
        Je nutné souhlasit se zpracováním osobních údajů.
      </div>
      <div class="gdpr-consent-button">
        OK
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'GDPRconsent'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global";

  .gdpr-consent {
    position: fixed;
    top: 0;
    left: 0;
    background: $orange;
    width: 100%;
    height: 80px;
    z-index: 100;
  }

  .gdpr-consent-inner {
    max-width: 770px;
    height: 100%;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .gdpr-consent-text {
    color: #fff;
    font-family: $nexa;
    font-size: 18px;
    font-weight: 300;
    letter-spacing: 0.72px;
  }

  .gdpr-consent-button {
    color: #fff;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.64px;
    padding: 13px 22px;
    border: 1px solid #fff;
    border-radius: 4px;
    box-sizing: border-box;
    cursor: pointer;
    transition: .3s background, .3s color;

    &:hover {
      background: #fff;
      color: $orange;
    }
  }

</style>
