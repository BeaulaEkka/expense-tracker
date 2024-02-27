<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Petrol", amount: -75 },
  { id: 3, text: "Book", amount: -16 },
  { id: 4, text: "Salary", amount: 3000 },
]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

//get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//get total income
const totalIncome = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});
//get total expenses
const totalExpenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//Add Transactions
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  toast.success("Transaction Added");
};
//generate unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
//delete Transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
  toast.success("Transaction Deleted");
};

//save to localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <Header />
  <Balance :total="total" />
  <IncomeExpenses :totalIncome="+totalIncome" :totalExpenses="+totalExpenses" />
  <TransactionList
    :transactions="transactions"
    @transactionDeleted="handleTransactionDeleted"
  />
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
</template>
