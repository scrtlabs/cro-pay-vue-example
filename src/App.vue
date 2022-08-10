<template>
    <CroPay
        @onApprove="paymentApproved"
        :address="walletAddress"
        :mint-amount="mintAmount"
        :payment-amount="usdPrice"
        payment-currency="USD"
        product-description="NFT"
    />
</template>

<script>
import CroPay from "./components/CroPay.vue";

export default {
    name: "App",
    components: {
        CroPay,
    },
    data() {
        return {
            walletAddress: "",
            mintAmount: 1,
            usdPrice: 100,
        };
    },

    mounted() {
        let croScript = document.createElement("script");
        croScript.setAttribute(
            "src",
            `https://js.crypto.com/sdk?publishable-key=${process.env.VUE_APP_CRYPTO_COM_PK}`
        );
        document.head.appendChild(croScript);
    },
    methods: {
        paymentApproved(data) {
            this.paymentId = data.id;
            if (data.status == "succeeded") {
                // Do something here
            }
        },
    },
};
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
