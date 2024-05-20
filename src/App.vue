<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpenses :income="income" :expenses="expenses"/>
    <TransationList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>

</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransationList from './components/TransationList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { useToast } from 'vue-toastification';

  import { ref, computed, onMounted } from 'vue'

  const toast = useToast();
  
  const transactions = ref([
    {id: 1, text:'Flower',amount: -19.99 },
    {id: 2, text:'Salary',amount: 299.97 },
    {id: 3, text:'Book',amount: -10 },
    {id: 4, text:'Camera',amount: 150 },
  ]);
  onMounted(() => {
    const saveTransation = JSON.parse(localStorage.getItem('transations'));

    if(saveTransation){
      transactions.value = transactions;
    }
  })

  const handleTransationSubmit = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount

    })

    saveTransactionTolocalStorage();

    toast.success('transaction added')

  }

  const saveTransactionTolocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000)
  }

  

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
  });

  const income = computed(() => {
    return transactions.value.
    filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
  });

  const expenses = computed(() => {
    return transactions.value.
    filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
  });
  
</script>