<template>
	<Header />
	<div class="container">
	<Balance :total="+total" />
	<IncomExpense :income="+income" :expenses="+expenses" />
	<TransactionList :transactions="transactions" 
	@transaction-deleted="handleTransactionDeleted"/>
	<AddTransaction @transaction-submitted="handleTransactionSubmitted"/>
	</div>
</template>


<script setup>
	import Header from './components/Header.vue'
	import Balance from './components/Balance.vue'
	import IncomExpense from './components/IncomExpense.vue'
	import TransactionList from './components/TransactionList.vue'
	import AddTransaction from './components/AddTransaction.vue'
	import { ref, computed, onMounted } from 'vue'
	import { generateCodeFrame } from 'vue/compiler-sfc'
	import { useToast } from 'vue-toastification'

	const toast = useToast();

	const transactions = ref( []);

	onMounted(() => {
		const savedTransactions = JSON.parse(localStorage.getItem
		('transactions'));

		if (savedTransactions) {
			transactions.value = savedTransactions;
		}
	});
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
	});
	const expenses = computed(() => {
		  return transactions.value
		  .filter((transaction) => transaction.amount < 0)
		  .reduce((acc, transaction) => {
		  		 return acc + transaction.amount;
		  }, 0)
		  .toFixed(2);
	});
	const handleTransactionSubmitted = (transactionData) => {
		//console.log(transactionData);
		transactions.value.push({ // push it into transactions
			id: generateId(),
			text: transactionData.text,
			amount: transactionData.amount,
		});
		saveTransactionsToLocal();

		toast.success('Transaction added');
	};
	const generateId = () => {
		return Math.floor(Math.random() * 100);
	};

	const handleTransactionDeleted = (id) => {
		transactions.value = transactions.value.filter(
			(transaction) => transaction.id !== id
		);
		saveTransactionsToLocal();
		toast.success('Transaction deleted');
	};

	const saveTransactionsToLocal = () => {
		localStorage.setItem('transactions', JSON.stringify(transactions.value));
	};	
//export default {
//	   components: {
//	   	   Header,
//		   Balance,
//		   IncomExpense,
//		   TransactionList,
//		   AddTransaction,
//		   //transactions,	  
//	   },
//};
</script>