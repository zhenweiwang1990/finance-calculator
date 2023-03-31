<template>
  <h-single-layout mode="middle" :width="1280">
    <div>
      <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
        <a-form-item label="Total Liabilities:">
          <h-input v-model:value="totalLiabilities" :save-options="{key:'totalLiabilities', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Total Debts:">
          <h-input v-model:value="totalDebts" :save-options="{key:'totalDebts', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Total income:">
          <a-input v-model:value="totalIncome" :save-options="{key:'totalIncome', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Calculate:">
          <a-button type="primary" @click="calculateLeverageRatio">Calculate Leverage Ratio</a-button>
        </a-form-item>
      </a-form>
      <h-singleline :value="leverageRatio" title="Leverage Ratio" size="large"/>
    </div>
  </h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from "financejs";
const finance = new Finance();
export default {
  setup() {
    const totalLiabilities = ref(25000);
    const totalDebts = ref(10000);
    const totalIncome = ref(20000);
    const leverageRatio = ref("");

    const calculateLeverageRatio = () => {

      leverageRatio.value = finance.LR(
        parseFloat(totalLiabilities.value), 
        parseFloat(totalDebts.value),
        parseFloat(totalIncome.value),
      )
    };

    return { totalLiabilities, totalIncome, leverageRatio, calculateLeverageRatio };
  },
};
</script>
