<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <span id="money-plus" class="money plus">+${{income}}</span>
    </div>
    <div>
      <h4>Expense</h4>
      <span id="money-minus" class="money minus">-${{expense.replace("-", "")}}</span>
    </div>
  </div>
</template>

<script setup>
import {defineProps} from "vue";

const props = defineProps({
  income: {
    type: String,
    required: true
  },
  expense: {
    type: String,
    required: true
  }
});

</script>
