<template>
  <h-single-layout mode="middle" :width="1280">
    <div>
      <h-multiline-input
        v-model="cashflows"
        title="Cashflows(list by lines)"
        placeholder="Enter cashflows, one per line"
        auto-select
        :save-options="{key:'cashflows', autoSave: true}"
      />
      <a-space>
        <a-button type="primary" @click="calculateIRR">Calculate IRR</a-button>
      </a-space>
      <h-singleline :value="irrResult" title="IRR" size="large"/>
    </div>
  </h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from 'financejs'
const finance = new Finance();

export default {
  setup() {
    const cashflows = ref(`-500000
200000
300000
200000`);
    const irrResult = ref("");

    const calculateIRR = () => {
      const lines = cashflows.value.trim().split("\n");
      const cashflowsArray = lines.map(line => parseFloat(line));

      irrResult.value = finance.IRR(...cashflowsArray).toFixed(3);
    };

    return { cashflows, irrResult, calculateIRR };
  },
};
</script>
