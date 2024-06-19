<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" id="text" type="text" placeholder="Cash" required/>
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input v-model="amount" type="text" id="amount" placeholder="100" required>
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import {ref} from 'vue';
import {useToast} from 'vue-toastification'

const text = ref("");
const amount = ref("");

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast();

const onSubmit = () => {
  if (!text.value?.trim() || !amount.value?.trim()) {
    toast.error("Please fill in all required fields");
    return;
  }

  if (Number.isNaN(parseFloat(amount.value))) {
    toast.error("Please fill in a valid number");
    document.getElementById('amount').focus();
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit("transactionSubmitted", transactionData);

  text.value = '';
  amount.value = '';
}
</script>
