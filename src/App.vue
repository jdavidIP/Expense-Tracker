<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([
  { id: 1, text: "Flower", amount: -18.99 },
  { id: 2, text: "Salary", amount: 1998.99 },
  { id: 3, text: "Video Game", amount: -105.29 },
  { id: 4, text: "Gift", amount: 50.0 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
}, 0);

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
}, 0);

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  console.log(id);

  toast.success("Transaction deleted");
};

const handleNewTransaction = (transactionData) => {
  transactions.value.push({
    id: generateID(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  toast.success("Transaction added");
};

const generateID = () => {
  return Math.floor(Math.random() * 100000);
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      @transactionDeleted="handleDeleteTransaction"
      :transactions="transactions"
    />
    <AddTransaction @transactionSubmitted="handleNewTransaction" />
  </div>
</template>
