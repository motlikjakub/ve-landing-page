<template>
  <section class="get-grant" id="request-grant">
    <div class="get-grant__dots get-grant__dots--top"></div>
    <div class="get-grant__container container">
      <h1 class="get-grant__heading">Zjistěte zdarma a jednoduše, <br> na jak vysokou dotaci máte nárok</h1>
      <form class="get-grant-form" action="">
        <div class="get-grant-form__row">
          <div>
            <label class="get-grant-form__label" for="get-grant-address">Adresa</label>
          </div>
          <div>
            <input class="get-grant-form__input" id="get-grant-address" type="text" v-model="grant_address" disabled>
          </div>
          <div>
            <a class="get-grant-form__link" href="#address" v-on:click="scroll('#address')">Zadat novou adresu</a>
          </div>
        </div>
        <div class="get-grant-form__row">
          <div>
            <label class="get-grant-form__label" for="get-grant-email">E-mail</label>
          </div>
          <div>
            <input class="get-grant-form__input" id="get-grant-email" v-model="email_address" type="email">
          </div>
        </div>
        <div class="get-grant-form__row">
          <div>
            <label class="get-grant-form__label" for="get-grant-text">Váš text</label>
          </div>
          <div>
            <textarea class="get-grant-form__input get-grant-form__input--textarea" id="get-grant-text" v-model="note"></textarea>
            <div class="get-grant-form-file">
              <input class="get-grant-form-file__input" id="get-grant-file" type="file" accept="image/*,.pdf">
              <div class="get-grant-form-file__half">
                <label class="get-grant-form-file__label" for="get-grant-file">Vaše roční vyúčtovací faktura <br>za elektrickou energii</label>
              </div>
              <div class="get-grant-form-file__half get-grant-form-file__half--with-button">
                <div class="get-grant-form-file__button">
                  Nahrát soubor
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="get-grant-form__row get-grant-form__row--with-button">
          <input id="get-grant-gdpr" class="get-grant-form-checkbox" type="checkbox"><label for="get-grant-gdpr" class="get-grant-form-checkbox-label"><span>Souhlasím se <a href="https://ve.solar/osobni-udaje" target="_blank">zpracováním osobních údajů</a></span></label>
          <Button @click.native="submitForm" text="Odeslat" classNames="greenish-button--smaller"/>
        </div>
      </form>
    </div>
    <div class="get-grant__dots get-grant__dots--bottom"></div>
  </section>
</template>

<script>
import axios from 'axios'
import Button from './Button'
var VueScrollTo = require('vue-scrollto')

export default {
  name: 'GetGrant',
  props: {
    grant_address: String,
    address_data: {}
  },
  components: {
    Button
  },
  data () {
    return {
      email_address: '',
      note: '',
    }
  },
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
    },
    submitForm: function () {
      let addressFormData = this.address_data;

      const formData = new FormData();
      formData.append('address[code]', addressFormData['code']);
      formData.append('address[latitude]', addressFormData.latitude);
      formData.append('address[longitude]', addressFormData.longitude);
      formData.append('address[buildingNumber]', addressFormData.buildingNumber);
      formData.append('address[city]', addressFormData.city);
      formData.append('address[zipCode]', addressFormData.zipCode);
      formData.append('address[district]', addressFormData.district);
      formData.append('address[region]', addressFormData.region);
      formData.append('address[country]', addressFormData.country);
      formData.append('address[street]', addressFormData.street);
      formData.append('address[cityPart]', addressFormData.cityPart);

      formData.append('email', this.email_address);
      formData.append('note', this.note);

      let inputFiles = document.querySelector('#get-grant-file');
      formData.append("invoice", inputFiles.files[0]);

      axios.post('https://vesolar.adwellqq.cz/api/vase-elektrarna/submit-offer', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log(error)
          alert('Při odesílání dat na server nastala chyba, zkuste to znovu později.')
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import "../scss/global";

  .get-grant {
    position: relative;
    width: 100%;
    min-height: 100vh;
    background: #dfe5ec;
    overflow-x: hidden;
  }

  .get-grant__dots {
    display: none;

    @media only screen and (min-width: $screen-md) {
      display: block;
	  }

    &--top {
      background: url("../assets/dots-top.svg");
      background-repeat: repeat-x;
      width: 1336px;
      height: 70px;
      margin: 0 auto;
      margin-top: 10px;
    }

    &--bottom {
      background: url("../assets/dots-bottom.svg");
      background-repeat: repeat-x;
      margin-bottom: 10px;
      width: 100%;
      height: 70px;
    }
  }

  .get-grant__container {
    max-width: 100%;
    padding-top: 60px;
    padding-bottom: 20px; // Form row has 10px bottom margin
    box-sizing: border-box;

    @media only screen and (min-width: $screen-md) {
      padding-bottom: 40px;
	  }
  }

  .get-grant__heading {
    color: $rich-black;
    font-family: $nexa;
    font-size: 32px;
    font-weight: 300;
    line-height: 42px;
    letter-spacing: 0.8px;
    margin-top: 0;
    margin-bottom: 35px;
    text-align: center;

    @media only screen and (min-width: $screen-md) {
      font-size: 40px;
      line-height: 54px;
      margin-bottom: 55px;
	  }
  }

  .get-grant-form {
    max-width: 500px;
    margin: 0 auto;
  }

  .get-grant-form__row {
    margin-bottom: 20px;

    &--with-button {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }
  }

  .get-grant-form__label {
    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 700;
    line-height: 29.19px;
    letter-spacing: 0.56px;
    margin-bottom: 9px;
  }

  .get-grant-form__input {
    border-radius: 4px;
    border: 1px solid #bec3ca;
    background-color: #fff;
    padding: 14px 22px;
    box-sizing: border-box;
    width: 100%;
    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 1;
    letter-spacing: 0.56px;
    height: 50px;
    outline: none;

    &--textarea {
      max-width: 100%;
      min-width: 100%;
      min-height: 110px;
      line-height: 1.5;
    }

    &:disabled {
      cursor: not-allowed;
    }
  }

  .get-grant-form__link {
    display: inline-block;
    color: $greenish;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 1;
    text-decoration: underline;
    letter-spacing: 0.56px;
    margin-top: 12px;

    &:hover {
      text-decoration: none;
    }
  }

  .get-grant-form-file {
    margin-top: 8px;
    width: 100%;
    border-radius: 4px;
    background: #c7cfd9;
    position: relative;
    padding: 20px 20px;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    transition: .3s background;

    @media only screen and (min-width: $screen-md) {

	  }

    &:hover {
      //background: #d3dae3;

      .get-grant-form-file__button {
        color: #fff;
        background: $rich-black;
      }
    }
  }

  .get-grant-form-file__input {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    outline: none;
    opacity: 0;
    cursor: pointer;
  }

  .get-grant-form-file__half {
    width: 100%;
    margin-bottom: 15px;

    @media only screen and (min-width: $screen-md) {
      width: 50%;
      margin-bottom: 0;
	  }

    &--with-button {
      text-align: center;
      margin-bottom: 0;

      @media only screen and (min-width: $screen-md) {
        text-align: right;
	    }
    }
  }

  .get-grant-form-file__label {
    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 700;
    line-height: 24px;
    letter-spacing: 0.56px;
  }

  .get-grant-form-file__button {
    display: block;
    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 700;
    line-height: 1;
    letter-spacing: 0.7px;
    padding: 17px 22px;
    box-sizing: border-box;
    border-radius: 4px;
    border: 1px solid $rich-black;
    transition: .3s color, .3s background;

    @media only screen and (min-width: $screen-md) {
      display: inline-block;
      padding: 20px 22px;
	  }
  }

  .get-grant-form-checkbox {
    display: none;
    opacity: 0;

    // Box hover
    &:hover + .get-grant-form-checkbox-label::before {
      background: $greenish;
    }

    // Box checked
    &:checked + .get-grant-form-checkbox-label::before {
      background: $greenish;
    }

    // Disabled state label.
    &:disabled + .get-grant-form-checkbox-label {
      color: #b8b8b8;
      cursor: auto;
    }

    // Disabled box.
    &:disabled + .get-grant-form-checkbox-label::before {
      background: #ddd;
    }

    // Checkmark. Could be replaced with an image
    &:checked + .get-grant-form-checkbox-label::after {
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

  .get-grant-form-checkbox-label {
    position: relative;
    cursor: pointer;
    padding: 0;

    color: $rich-black;
    font-family: $nexa;
    font-size: 14px;
    font-weight: 300;
    line-height: 22px;
    letter-spacing: 0.56px;
    display: flex;
    align-items: flex-start;
    margin-bottom: 30px;

    @media only screen and (min-width: $screen-md) {
      align-items: center;
      margin-bottom: 0;
	  }

    a {
      color: $greenish;

      &:hover {
        text-decoration: none;
      }
    }

    &::before {
      content: '';
      margin-right: 10px;
      display: block;
      width: 20px;
      min-width: 20px;
      height: 20px;
      background: white;
      border-radius: 4px;
      border: 1px solid #bec3ca;
      transition: .2s;
    }
  }

</style>
