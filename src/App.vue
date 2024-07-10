<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses  :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" 
    @deleteTransaction="handleTransactionsDeleted" />
    <AddTransaction @transactionsSubmitted="handleTransactionsSubmitted"/>
  </div>
</template>


<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { ref, computed, onMounted } from 'vue';

  import { useToast } from 'vue-toastification';

  const toast = useToast();

  const transactions = ref([]);

  onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

  // Получаем сумму общего баланса

  const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});
  // Получаем сумму дохода
  const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
  });
  // Получаем сумму расходов
  const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
  });
  // Добавление транзакции в историю
  const handleTransactionsSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    });
    saveTransactionsToLocalStorage();
    toast.success("Транзакция добавлена");
  };
  // Генерация уникального id
  const generateUniqueId = () => {
    return Math.floor(Math.random() *100000)
  }
  // Удалить транзакцию
  const handleTransactionsDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );
    saveTransactionsToLocalStorage();
    toast.success('Транзакция удалена');
  };
  // Добавление транзакции в локальное хранилище
  const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};  
</script>