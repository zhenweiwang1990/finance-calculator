<template>
  <h-single-layout mode="middle" :width="600">
    <div>
      <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
        <a-form-item label="Rate (%):">
          <h-input v-model:value="rate" :save-options="{key:'rate', autoSave: true}" />
        </a-form-item>
        <a-form-item label="Cashflow:">
          <h-input v-model:value="cashflow" :save-options="{key:'cashflow', autoSave: true}" />
        </a-form-item>
        <a-form-item label="Number of Periods:">
          <a-input-number v-model:value="periods" :min="1" :save-options="{key:'periods', autoSave: true}" />
        </a-form-item>
        <a-form-item label="Calculate:">
          <a-button type="primary" @click="calculateFutureValue">Calculate Future Value</a-button>
        </a-form-item>
      </a-form>
      <h-singleline :value="futureValue" title="Future Value" size="large" />
    </div>
  </h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from "financejs";
const finance = new Finance();

export default {
  setup() {
    const rate = ref(5);
    const cashflow = ref(1000);
    const periods = ref(5);
    const futureValue = ref("");

    const calculateFutureValue = () => {
      futureValue.value = finance.FV(
        parseFloat(rate.value),
        parseFloat(cashflow.value),
        periods.value
      ).toFixed(3);
    };

    return { rate, cashflow, periods, futureValue, calculateFutureValue };
  },
};
</script>
