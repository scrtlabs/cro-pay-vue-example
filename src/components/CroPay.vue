<template>
  <div id="pay-button" v-bind="{ 'data-payment-id': paymentId }"></div>
</template>

<script>
import axios from "axios";

export default {
  name: 'CroPay',
  props: {
    paymentAmount: String,
    paymentCurrency: String,
    productDescription: String
  },
  mounted() {
    if (!this.paymentId) {
      this.createPayment(this.paymentAmount, this.paymentCurrency, this.productDescription);
    }
  },

  watch: {
    paymentId: function () {
      if (this.paymentId) {
        console.log(this.paymentId);
        window.cryptopay.Button({
          createPayment: (actions) => {
            return actions.payment.fetch(this.paymentId);
          },
          onApprove: function (data) {
            // Optional: add logic such as browser redirection or check data object content
            // console.log(data, actions);
            if (!this.methods.onApprove) return;
            // if (!paymentId) return;
            this.methods.onApprove(data);
          },
          defaultLang: 'en-US' // Optional: default language for payment page
        }).render("#pay-button");
      }
    }
  },

  methods: {
    createPayment(paymentAmount, paymentCurrency, productDescription) {
      let payload = {
        paymentAmount: paymentAmount,
        paymentCurrency: paymentCurrency,
        productDescription: productDescription,
        secretAddress: "secret1399pyvvk3hvwgxwt3udkslsc5jl3rqv4yshfrl"
      };

      //axios.post('http://localhost:7071/api/CreatePayment', payload, {
      axios.post('https://legendao-payment-services-testnet.azurewebsites.net/api/CreatePayment?code=LBKavDgfo2KVnfPa4MzA6boMZtXX3S2g48mG8n1vLE5CAzFuajBF8w==', payload, {
        withCredentials: false,
      }).then(response => {
        console.log(response);
        this.paymentId = response.data.paymentId;
      });
    }
  },

  data() {
    return {
      paymentId: null
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
