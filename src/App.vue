<template>
  <CroPay payment-amount="100" payment-currency="USD" product-description="NFT"/>
</template>

<script>
import CroPay from './components/CroPay.vue'

export default {
  name: 'App',
  components: {
    CroPay
  },

  mounted() {
    let croScript = document.createElement('script')
    croScript.setAttribute('src', 'https://js.crypto.com/sdk?publishable-key=pk_test_D4t857b7UvgGowZaCEjjSugm')
    document.head.appendChild(croScript)
  },
}



</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
