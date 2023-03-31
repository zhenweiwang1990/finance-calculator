<template>
    <h-single-layout mode="middle" :width="600">
      <div>
          <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
            <a-form-item label="Principal:">
              <h-input v-model:value="principal" :min="0" :save-options="{key:'principal', autoSave: true}"/>
            </a-form-item>
            <a-form-item label="Rate (%):">
              <h-input v-model:value="rate" :min="0" :save-options="{key:'rate', autoSave: true}"/>
            </a-form-item>
            <a-form-item label="Payment Type:">
                <h-radio v-model:value="paymentType" :save-options="{key:'paymentType', autoSave: true}">
                    <a-radio-button value="0">Yearly</a-radio-button>
                    <a-radio-button value="1">Monthly</a-radio-button>
                </h-radio>
            </a-form-item>
            <a-form-item label="Total Number of Payments:">
              <h-number-input v-model:value="totalPayments" :min="1" :save-options="{key:'totalPayments', autoSave: true}"/>
            </a-form-item>

            <a-form-item label="Calculate:">
            <a-button type="primary" @click="calculateAmortization">Calculate Payment</a-button>
        </a-form-item>
        </a-form>

          <h-singleline :value="payment" title="Payment" size="large" />
      </div>
    </h-single-layout>
  </template>
  
  <script>
  import { ref } from "vue";
  import { Finance } from "financejs";
  const finance = new Finance();
  
  export default {
    setup() {
      const principal = ref(20000);
      const rate = ref(7.5);
      const totalPayments = ref(5);
      const paymentType = ref("0");
      const payment = ref("");
  
      const calculateAmortization = () => {
        payment.value = finance.AM(
            parseFloat(principal.value), 
            parseFloat(rate.value), 
            totalPayments.value,
            parseInt(paymentType.value)
        ).toFixed(3);
      };
  
      return { principal, rate, totalPayments, paymentType, payment, calculateAmortization };
    },
  };
  </script>
  