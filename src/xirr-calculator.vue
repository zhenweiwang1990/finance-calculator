<template>
  <h-single-layout mode="middle" :width="1280">
    <div>
        <h-multiline-input
          v-model="cashflowCsv"
          title="Cashflows (CSV)"
          placeholder="Date,Amount"   
          auto-select
          :save-options="{key:'cashflows', autoSave: true}"
        />
      <a-space style="margin-top: 20px">
        <span>Total Asset Value(Present Value):</span>
        <h-number-input v-model:value="totalAssetValue" :min="0"           
        :save-options="{key:'totalAssetValue', autoSave: true}"
        />
        
        <span>Guess(Optional):</span>
        <h-input v-model="guess" style="width: 60px;"        
        :save-options="{key:'guess', autoSave: true}"
        />
        <a-button type="primary" @click="calculateXIRR">Calculate XIRR</a-button>
      </a-space>
      <h-singleline :value="xirrV" title="XIRR" size="large"/>
  </div>
</h-single-layout>
</template>

<script>
import { ref } from "vue";
import { Finance } from 'financejs'
const finance = new Finance();

export default {
  setup() {
    const cashflowCsv = ref(`2015-11-01,-1000
2016-07-01,-100
2016-07-19,1200
`);
    const totalAssetValue = ref(0);
    const xirrV = ref("");
    const guess = ref(0.1);
    const calculateXIRR = () => {
      const lines = cashflowCsv.value.trim().split("\n");
      const cashflows = lines.map(line => {
          return parseFloat(line.split(",")[1]);
      });
      cashflows.push(totalAssetValue.value)
      const dates = lines.map(line => {
          return new Date(line.split(",")[0]);
      });
      dates.push(new Date())
      debugger
      xirrV.value = finance.XIRR(cashflows, dates, guess.value).toFixed(3);
    };
    return { cashflowCsv, totalAssetValue, xirrV, calculateXIRR };
  },
};
</script>
