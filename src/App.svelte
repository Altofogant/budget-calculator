<script>
	import Navbar from './Navbar.svelte';
	import ExpensesData from './expenses.js';
	import ExpensesList from './ExpensesList.svelte';
	import { setContext } from 'svelte';

	let expenses = [...ExpensesData];

	function removeExpense(id) {
		expenses = expenses.filter(item => item.id !== id);
	}

	function clearExpenses() {
		expenses = [];
	}

	setContext('remove', removeExpense);
</script>

<Navbar />
<main class="content">
	<ExpensesList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses} >
		Clear Expenses
	</button>
</main>