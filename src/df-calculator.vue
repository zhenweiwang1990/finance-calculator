<template>
  <h-single-layout mode="middle" :width="600">
    <div>
      <a-form :label-col="{ span: 8 }" :wrapper-col="{ span: 18 }">
        <a-form-item label="Rate (%):">
          <h-input v-model:value="rate" :save-options="{key:'rate', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Number of Periods:">
          <h-number-input v-model:value="periods" :min="1" :save-options="{key:'periods', autoSave: true}"/>
        </a-form-item>
        <a-form-item label="Calculate:">
          <a-button type="primary" @click="calculateDiscountFactors">Calculate Discount Factors</a-button>
        </a-form-item>
      </a-form>

      <a-table :data-source="discountFactors" :columns="columns" />

    </div>
  </h-single-layout>
</template>

<script>
import { ref,defineComponent } from "vue";
import { Finance } from "financejs";
const finance = new Finance();

const rate = ref(5);
const periods = ref(5);
const discountFactors = ref([]);

const columns = [
  {
    title: "Period",
    dataIndex: "period",
  },
  {
    title: "Discount Factor",
    dataIndex: "discountFactor",
  },
];

const calculateDiscountFactors = () => {
  const dfs = finance.DF(parseFloat(rate.value), periods.value)
  discountFactors.value = dfs.map( (df,index) => {
    return {
      period: index,
      discountFactor: df.toFixed(4),
    }
  });
  debugger
};

export default defineComponent({
  setup() {
    return { rate, periods, discountFactors, columns, calculateDiscountFactors };
  },
});
</script>
