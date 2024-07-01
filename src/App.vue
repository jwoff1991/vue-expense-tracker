<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>


<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import {ref, computed} from 'vue'

  const transactions= ref([
          { id: 1, text: 'Flowers', amount: -19.99 },
          { id: 2, text: 'Salary', amount: 300 },
          { id: 3, text: 'Book', amount: -10 },
          { id: 4, text: 'Camera', amount: 150 }
  ])

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
  })

  const income = computed(() => {
    return transactions.value.filter(transaction => transaction.amount > 0).reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
  })

  const expense = computed(() => {
    return transactions.value.filter(transaction => transaction.amount < 0).reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0).toFixed(2)
  })

// Submit transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success('Transaction added.');
};

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};


const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted.');
};
</script>
