<template>
  <section class="address-section" id="address">
    <img class="logo" alt="Vaše elektrárna logo" src="../assets/VE-logo.svg">
    <div class="address-section__inner">
      <h1 class="address-section-text">
        Zadejte svoji adresu <br>
        a zjistěte, kolik vyrobíte energie.
      </h1>
      <form class="address-form container">
        <div class="address-form__row">
          <label for="address-input-address" class="address-form-input-label">Ulice a město</label>
        </div>
        <div class="address-form__row">
          <input id="address-input-address" class="address-form-input smartform-whole-address" type="text" placeholder="Ulice a město">
          <Button link="#grant" classNames="checkbox-required" text="Spočítat"/>
        </div>
        <div class="address-form__row">
          <input id="address-input-agree" class="address-form-checkbox" type="checkbox"><label for="address-input-agree" class="address-form-checkbox-label"><span>Souhlasím se <a href="">zpracováním osobních údajů</a></span></label>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import Button from './Button'

export default {
  name: 'AddressSection',
  components: {
    Button
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global";

  .address-section {
    position: relative;
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: url("../assets/address-bcg.jpg");
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    padding-top: 100px;
    padding-bottom: 50px;
    box-sizing: border-box;
  }

  .address-section__inner {
    text-align: center;
  }

  .address-section-text {
    color: #fff;
    font-family: $nexa;
    font-size: 45px;
    line-height: 60px;
    font-weight: 300;
    letter-spacing: 1.8px;
    text-align: center;
    margin-bottom: 45px;
    padding: 0 20px;
    box-sizing: border-box;

    @media only screen and (min-width: $screen-md) {
      font-size: 50px;
      line-height: 70px;
      margin-bottom: 70px;
	  }

    @media only screen and (min-width: $screen-lg) {
      font-size: 90px;
      line-height: 110px;
      margin-bottom: 95px;
	  }
  }

  // Form styles
  .address-form {
    text-align: left;
    display: block;

    @media only screen and (min-width: $screen-md) {
      display: inline-block;
	  }
  }

  .address-form__row {
    display: flex;
    align-items: center;
    margin-bottom: 18px;
    flex-wrap: wrap;
  }

  .address-form-input-label {
    color: #fff;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    line-height: 1;
    letter-spacing: 0.72px;

    @media only screen and (min-width: $screen-lg) {
      font-size: 18px;
	  }
  }

  .address-form-input {
    padding: 16px 19px;
    height: 45px;
    width: 100%;
    box-sizing: border-box;
    background: #fff;
    border: none;
    border-radius: 4px;
    outline: none;

    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 1;
    letter-spacing: 0.54px;
    margin-bottom: 20px;

    @media only screen and (min-width: $screen-md) {
      font-size: 16px;
      padding: 22px 32px;
      font-size: 18px;
      width: 560px;
      height: 60px;
      margin-right: 30px;
      margin-bottom: 0;
	  }

    @media only screen and (min-width: $screen-lg) {
      font-size: 18px;
      height: 70px;
      width: 600px;
	  }
  }

  .address-form-checkbox {
    opacity: 0;
    width: 10px;
    margin-left: -10px;

    // Box checked
    &:checked + .address-form-checkbox-label::before {
      background: $greenish;
    }

    // Disabled state label.
    &:disabled + .address-form-checkbox-label {
      color: #b8b8b8;
      cursor: auto;
    }

    // Disabled box.
    &:disabled + .address-form-checkbox-label::before {
      background: #ddd;
    }

    // Checkmark. Could be replaced with an image
    &:checked + .address-form-checkbox-label::after {
      content: '';
      position: absolute;
      left: 5px;
      top: 9px;
      background: white;
      width: 2px;
      height: 2px;
      box-shadow:
        2px 0 0 white,
        4px 0 0 white,
        4px -2px 0 white,
        4px -4px 0 white,
        4px -6px 0 white,
        4px -8px 0 white;
      transform: rotate(45deg);
    }
  }

  .address-form-checkbox-label {
    position: relative;
    display: flex;
    cursor: pointer;
    padding: 0;

    color: #fff;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 22px;
    letter-spacing: 0.56px;

    a {
      color: #fff;

      &:hover {
        text-decoration: none;
      }
    }

    &::before {
      content: '';
      display: block;
      margin-right: 10px;
      vertical-align: text-top;
      width: 20px;
      min-width: 20px;
      height: 20px;
      background: white;
      border-radius: 4px;
      transition: .2s;
    }
  }

  @keyframes red-blink {
    0% {
      color: #fff;
    }
    50% {
      color: #d63c1d;
    }
    100% {
      color: #fff;
    }
  }

</style>

<style lang="scss">
  @import "../scss/global";

  .gwt-SuggestBoxPopup {
    font-family: $nexa;
    border-bottom-right-radius: 4px;
    border-bottom-left-radius: 4px;
    overflow: hidden;

    .item.item-selected {
      background: #8aedea;
      cursor: pointer;
    }

    .suggestPopupContent {
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
    }
  }

  .gwt-Anchor-suggestLink  {
    //display: none;
    color: $greenish;
    font-size: 12px;
  }
</style>