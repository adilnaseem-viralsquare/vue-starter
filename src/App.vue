<!-- <template>
  <header>
   
    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>
  My App
  <RouterView />
</template> -->

<template>
  My App
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList
      :transactions="transactions"
      @deleteTransaction="handleDelete"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmit" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";

import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

// <-------------------- no need to do this if we put setup in script tag ------------------------>
// export default {
//   components: {
//     Header,
//     Balance,
//     IncomeExpense,
//     TransactionList,
//     AddTransaction,
//   },
// };
const transactions = ref([]);
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) transactions.value = savedTransactions;
});

const toast = useToast();



const total = computed(() => {
  return transactions.value.reduce((acc, curr) => {
    return acc + curr.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .reduce((acc, curr) => {
      if (curr.amount > 0) return acc + curr.amount;
      return acc;
    }, 0)
    .toFixed(2);
});

const expense = computed(() => {
  return transactions.value
    .reduce((acc, curr) => {
      if (curr.amount < 0) return acc + curr.amount * -1;
      return acc;
    }, 0)
    .toFixed(2);
});

const handleTransactionSubmit = function (data) {
  transactions.value.push({ id: transactions.value.length + 1, ...data });
  toast.success("Transaction Added successfully");
  saveToLocalStorage();
};

const handleDelete = function (id) {
  transactions.value = transactions.value.filter((item) => item.id !== id);
  toast.error("Transaction deleted");
  saveToLocalStorage();
};

const saveToLocalStorage = function () {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
