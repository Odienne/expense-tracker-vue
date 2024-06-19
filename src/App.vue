<template>
  <Header/>
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income :expense/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleNewTransaction"/>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {useToast} from 'vue-toastification'
import {ref, computed, onMounted} from 'vue';

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
})

const toast = useToast();


let transactions = ref([
  {id: 1, text: 'Flower', amount: -19.99},
  {id: 2, text: 'Salary', amount: 1600},
  {id: 3, text: 'Books', amount: -33.99},
  {id: 4, text: 'Camera', amount: -389.99},
]);

//get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

//get income
const income = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0).toFixed(2);
});

//get expense
const expense = computed(() => {
  return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0).toFixed(2);
});

const handleTransactionDeleted = (id) => {
  const index = transactions.value.findIndex(transaction => transaction.id === id);
  transactions.value.splice(index, 1)

  updateLocalStorage();
  toast.success("Transaction deleted");
}

const handleNewTransaction = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    ...transactionData
  });

  updateLocalStorage();
  toast.success("Transaction added");
}

//save to local storage
const updateLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}
</script>
