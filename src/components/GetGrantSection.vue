<template>
  <section class="get-grant" id="request-grant">
    <GDPRconsent v-bind:show="gdpr_shown"/>
    <AlertBar v-bind:show="alert_bar_shown" :message="alert_message"/>
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
            <a class="get-grant-form__link" href="#address" v-on:click="scroll('#address')" @click="hideAlert()">Zadat novou adresu</a>
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
              <input class="get-grant-form-file__input" id="get-grant-file" type="file" accept="image/*,.pdf" ref="invoice_file" @change="invoiceSelected">
              <!-- <div class="get-grant-form-file--top" :class="{ 'is-shown': invoice_select_text }" v-html="invoice_select_text"></div>
              <div class="get-grant-form-file--bottom">
                <div class="get-grant-form-file__half">
                  <label class="get-grant-form-file__label" for="get-grant-file">Roční vyúčtovací faktura <br>za elektrickou energii</label>
                </div>
                <div class="get-grant-form-file__half get-grant-form-file__half--with-button">
                  <div class="get-grant-form-file__button">
                    <span v-if="invoice_selected">Změnit soubor</span>
                    <span v-else>Nahrát soubor</span>
                  </div>
                </div>
              </div> -->
            </div>
          </div>
        </div>
        <div class="get-grant-form__row get-grant-form__row--with-button">
          <input id="get-grant-gdpr" class="get-grant-form-checkbox" v-model="gdpr_accepted" type="checkbox"><label for="get-grant-gdpr" class="get-grant-form-checkbox-label"><span>Souhlasím se <a href="https://ve.solar/osobni-udaje" target="_blank">zpracováním osobních údajů</a></span></label>
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
import GDPRconsent from './GDPR-consent'
import AlertBar from './AlertBar'
import { APIendpoint } from '../variables.js'
var VueScrollTo = require('vue-scrollto')

export default {
  name: 'GetGrant',
  props: {
    grant_address: String,
    address_data: {}
  },
  components: {
    AlertBar,
    Button,
    GDPRconsent
  },
  data () {
    return {
      email_address: '',
      note: '',
      gdpr_accepted: false,
      gdpr_shown: false,
      invoice_selected: false,
      invoice_select_text: '',
      alert_bar_shown: false,
      alert_message: 'Pro tuto adresu již evidujeme dotaz na výši dotace'
    }
  },
  methods: {
    scroll: function (link) {
      VueScrollTo.scrollTo(link)
    },
    invoiceSelected: function () {
      if (this.$refs.invoice_file.files[0]) {
        this.invoice_selected = true
        this.invoice_select_text = 'Soubor <span style="font-weight: 400">' + this.$refs.invoice_file.files[0].name + '</span> nahrán'
      } else {
        this.invoice_selected = false
        this.invoice_select_text = 'Nahrajete-li soubor, Váš dotaz bude přesnější. <br>Pokud ne, nevadí, můžete pokračovat dále v odeslání.'
      }
    },
    validEmail: function (email) {
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email)
    },
    submitForm: function () {
      if (this.grant_address) {
        if (this.validEmail(this.email_address)) {
          if (this.gdpr_accepted) {
            let addressFormData = this.address_data

            const formData = new FormData()
            formData.append('address[code]', addressFormData['code'])
            formData.append('address[latitude]', addressFormData.latitude)
            formData.append('address[longitude]', addressFormData.longitude)
            formData.append('address[house_number]', addressFormData.houseNumber)
            formData.append('address[orientation_number]', addressFormData.orientationNumber)
            formData.append('address[city]', addressFormData.city)
            formData.append('address[zip]', addressFormData.zipCode)
            formData.append('address[district]', addressFormData.district)
            formData.append('address[region]', addressFormData.region)
            formData.append('address[country]', addressFormData.country)
            formData.append('address[street]', addressFormData.street)
            formData.append('address[city_part]', addressFormData.cityPart)

            formData.append('email', this.email_address)
            formData.append('note', this.note)

            let inputFiles = document.querySelector('#get-grant-file')
            formData.append('invoice', inputFiles.files[0])

            if ((inputFiles.files[0] || this.invoice_select_text) && true /* Delete this if using invoice upload */) {
              this.alert_message = 'Probíhá odesílání dat...'
              this.alert_bar_shown = true

              axios.post(APIendpoint + '/api/vase-elektrarna/submit-offer', formData, {
                headers: {
                  'Content-Type': 'multipart/form-data'
                }
              })
                .then(response => {
                  this.alert_message = 'Data byla úspěšně odeslána'
                  this.alert_bar_shown = true
                })
                .catch(error => {
                  if (process.env.NODE_ENV === 'development') { // Only if development
                    console.log(error)
                  }
                  if (error.response.status === 409) {
                    this.alert_message = 'Pro tuto adresu již evidujeme dotaz na výši dotace'
                    this.alert_bar_shown = true
                  } else {
                    this.alert_message = 'Při odesílání dat na server nastala chyba, zkuste to znovu později'
                    this.alert_bar_shown = true
                  }
                })
            } else {
              this.invoice_select_text = 'Nahrajete-li soubor, Váš dotaz bude přesnější. <br>Pokud ne, nevadí, můžete pokračovat dále v odeslání.'
            }
          } else {
            this.gdpr_shown = true
          }
        } else {
          this.alert_message = 'Email není validní'
          this.alert_bar_shown = true
        }
      } else {
        this.alert_message = 'Je nutné vyplnit adresu'
        this.alert_bar_shown = true
      }
    },
    hideAlert: function () {
      this.alert_bar_shown = false
    }
  },
  watch: {
    gdpr_accepted: function () {
      if (this.gdpr_accepted) {
        this.gdpr_shown = false
      } else {
        this.gdpr_shown = true
      }
    },
    email_address: function () {
      if (this.validEmail(this.email_address)) {
        this.alert_bar_shown = false
      } else {
        this.alert_message = 'Email není validní'
        this.alert_bar_shown = true
      }
    },
    grant_address: function () {
      if (this.grant_address) {
        this.alert_bar_shown = false
      }
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
    max-width: 600px;
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
    align-items: center;
    transition: .3s background;

    @media only screen and (min-width: $screen-md) {

	  }

    &:hover {
      //background: #737B85;

      .get-grant-form-file__button {
        color: #fff;
        background: $rich-black;
      }
    }
  }

  .get-grant-form-file--top {
    color: $rich-black;
    font-family: $nexa;
    font-size: 16px;
    font-weight: 700;
    line-height: 24px;
    letter-spacing: 0.64px;
    text-align: center;
    box-sizing: border-box;
    height: 0;
    padding: 0;
    overflow: hidden;
    transition: .3s;

    &.is-shown {
      height: auto;
      padding: 20px 22px;
      border-bottom: 1px solid #fff;
    }
  }

  .get-grant-form-file--bottom {
    padding: 20px 22px;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
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
