<template>
    <div>
        <div
            :class="myclass"
            id="pay-button"
            v-bind="{ 'data-payment-id': paymentId }"
        ></div>
        <div v-if="showPlaceholder" class="cro-placeholder">
            {{ error == "" ? "Loading..." : error }}
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "CroPay",
    props: {
        paymentAmount: String,
        paymentCurrency: String,
        productDescription: String,
        address: String,
        mintAmount: String,
    },
    mounted() {
        if (!this.paymentId) {
            // Just to make sure that the parent was fully loaded
            setTimeout(function () {
                this.createPayment(
                    this.paymentAmount,
                    this.paymentCurrency,
                    this.productDescription,
                    this.mintAmount
                );
            }, 1000);
        }
    },

    watch: {
        paymentId: function () {
            var self = this;

            if (this.paymentId) {
                // console.log(this.paymentId);
                window.cryptopay
                    .Button({
                        createPayment: (actions) => {
                            return actions.payment.fetch(this.paymentId);
                        },
                        onApprove: function (data) {
                            self.$emit("onApprove", data);
                        },
                        defaultLang: "en-US", // Optional: default language for payment page
                    })
                    .render("#pay-button");
            }
        },
    },

    methods: {
        createPayment(
            paymentAmount,
            paymentCurrency,
            productDescription,
            mintAmount
        ) {
            var self = this;

            if (this.address == "") {
                this.error = "Error, no address";
                return;
            }

            var paymentToSend = parseInt(mintAmount) * parseInt(paymentAmount);
            let payload = {
                paymentAmount: paymentToSend,
                paymentCurrency: paymentCurrency,
                productDescription: productDescription,
                secretAddress: this.address,
                mintAmount: parseInt(mintAmount),
            };
            try {
                axios
                    .post(
                        `${process.env.VUE_APP_CRYPTO_COM_REST_SERVER}/CreatePayment?code=${process.env.VUE_APP_CRYPTO_COM_AUTH_EP}`,
                        payload,
                        {
                            withCredentials: false,
                        }
                    )
                    .then((response) => {
                        self.showPlaceholder = false;
                        self.paymentId = response.data.paymentId;
                    });
            } catch (err) {
                console.error(err);
            }
        },
    },

    data() {
        return {
            myclass: "",
            paymentId: null,
            showPlaceholder: true,
            error: "",
        };
    },
};
</script>

<style scoped>
.cro-placeholder {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 16px;
    width: 230px;
    height: 50px;
    background-color: #002c74;
}
element.style {
    width: 230px;
    height: 50px;
    cursor: pointer;
}
</style>
