<script>
	import Navbar from './Navbar.svelte';
	import ExpensesData from './expenses.js';
	import ExpensesList from './ExpensesList.svelte';
	import { setContext } from 'svelte';
	import Totals from './Totals.svelte';
	import ExpenseForm from './ExpenseForm.svelte';

	let expenses = [...ExpensesData];
	$: total = expenses.reduce((prev, next) => {
		return (prev += next.amount);
	}, 0);

	function removeExpense(id) {
		expenses = expenses.filter(item => item.id !== id);
	}

	function clearExpenses() {
		expenses = [];
	}

	function addExpense({name, amount}) {
		let expense = {id: Math.random() * Date.now(), name, amount};
		expenses = [expense,...expenses];
	}

	setContext('remove', removeExpense);
</script>

<Navbar />
<main class="content">
	<ExpenseForm {addExpense}/>
	<Totals title="Total expenses" {total} />
	<ExpensesList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses} >
		Clear Expenses
	</button>
</main>