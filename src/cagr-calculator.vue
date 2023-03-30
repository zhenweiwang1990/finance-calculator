<template>
  <h-single-layout mode="middle" :width="600">
    <div>
      <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
        <a-form-item label="Beginning Value:">
          <h-input v-model:value="beginningValue" :save-options="{key:'beginningValue', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Ending Value:">
          <h-input v-model:value="endingValue" :save-options="{key:'endingValue', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Periods:">
          <a-input-number v-model:value="periods" :min="1" :save-options="{key:'periods', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Calculate:">
          <a-button type="primary" @click="calculateCAGR">Calculate CAGR</a-button>
        </a-form-item>
      </a-form>
      <h-singleline :value="cagr" title="CAGR" size="large" />
    </div>
  </h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from "financejs";
const finance = new Finance();

export default {
  setup() {
    const beginningValue = ref(10000);
    const endingValue = ref(19500);
    const periods = ref(3);
    const cagr = ref("");

    const calculateCAGR = () => {
      cagr.value = finance.CAGR(
        parseFloat(beginningValue.value),
        parseFloat(endingValue.value),
        parseInt(periods.value)
      ).toFixed(3);
    };

    return { beginningValue, endingValue, periods, cagr, calculateCAGR };
  },
};
</script>
