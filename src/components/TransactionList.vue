<template>
	<h3>History</h3>
	<ul id="list" class="list">
	  <li v-for="transaction in transactions"
	   v-bind:key="transaction.id"
	   :class="transaction.amount < 0 ? 'minus' : 'plus'">
	  	  {{transaction.text}} <span>${{transaction.amount}}</span><button @click="deleteTransaction(transaction.id)"
      	  class="delete-btn">x</button> 
	  </li>
		<!-- li class="minus">
			Cash <span>-$400</span><button
			class="delete-btn">x</button>
		</li>
		<li class="plus">
			Paycheck <span>$400</span><button
			class="delete-btn">x</button>
		</li> -->
	</ul>
</template>

<script setup>
//export default {

//ta() {
//	  return {
//	  	 transactions: [
//		 	{ id : 1, text: 'Flower', amount: -19.99},
//		 ],
//	  };
//

//	setup() {
//	  const transactions = [
//	  		{ id: 1, text: 'Flower', amount: -19.99 },
//	  ];
//
//	return {
//	  transactions,
//	};
//	},
//
//
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeleted']);

const props = defineProps({
	  transactions: {
		  type: Array,
		  required: true,
		},
});

const deleteTransaction = (id) => {
	emit('transactionDeleted', id);
};
</script>