<template>
  <h-single-layout mode="middle" :width="600">
    <div>
      <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
        <a-form-item label="Rate (%):">
          <h-input v-model:value="rate"        
             :save-options="{key:'rate', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Compoundings per Period:">
          <a-input-number v-model:value="compoundings" :min="1" :save-options="{key:'compoundings', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Principal:">
          <h-input v-model:value="principal" :save-options="{key:'principal', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Number of Periods:">
          <a-input-number v-model:value="periods" :min="1" :save-options="{key:'periods', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Calculate:">
          <a-button type="primary" @click="calculateCompoundInterest">Calculate Compound Interest</a-button>
        </a-form-item>
      </a-form>
      <h-singleline :value="compoundInterest" title="Compound Interest" size="large" />
    </div>
  </h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from "financejs";
const finance = new Finance();

export default {
  setup() {
    const rate = ref(5.2);
    const compoundings = ref(12);
    const principal = ref(1000);
    const periods = ref(5);
    const compoundInterest = ref("");

    const calculateCompoundInterest = () => {
      compoundInterest.value = finance.CI(
        parseFloat(rate.value),
        parseInt(compoundings.value),
        parseFloat(principal.value),
        parseInt(periods.value)
      ).toFixed(2);
    };

    return { rate, compoundings, principal, periods, compoundInterest, calculateCompoundInterest };
  },
};
</script>
