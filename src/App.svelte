<script>
	import Navbar from './Navbar.svelte';
	//import ExpensesData from './expenses.js';
	import ExpensesList from './ExpensesList.svelte';
	import { setContext, onMount } from 'svelte';
	import Totals from './Totals.svelte';
	import ExpenseForm from './ExpenseForm.svelte';

	let expenses = [];
	let setName = '';
	let setAmount = null;
	let setId = null;
	let isFormOpen = false;

	$: isEditing = setId ? true : false;
	$: total = expenses.reduce((prev, next) => {
		return (prev += next.amount);
	}, 0);

	function showForm() {
		isFormOpen = true;
	}

	function hideForm() {
		isFormOpen = false;
		setName = '';
		setAmount = null;
		setId = null;
	}

	function removeExpense(id) {
		expenses = expenses.filter(item => item.id !== id);
		setLocalStorage();
	}

	function clearExpenses() {
		expenses = [];
		setLocalStorage();
	}

	function addExpense({name, amount}) {
		let expense = {id: Math.random() * Date.now(), name, amount};
		expenses = [expense,...expenses];
		setLocalStorage();
	}

	function setModifiedExpense(id) {
		let expense = expenses.find(item => item.id === id);
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}

	function editExpense({name, amount}) {
		expenses = expenses.map(item => {
			return item.id === setId ? {...item, name, amount} : {...item};
		});
		setId = null;
		setAmount = null;
		setName = '';
		setLocalStorage();
	}

	setContext('remove', removeExpense);
	setContext('modify', setModifiedExpense);

	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}
	onMount(() => {
		expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : [];
	})
</script>

<Navbar {showForm} />
<main class="content">
	{#if isFormOpen}
		<ExpenseForm {addExpense} name={setName} amount={setAmount} {isEditing} {editExpense} {hideForm} />
	{/if}
	<Totals title="Total expenses" {total} />
	<ExpensesList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses} >
		Clear Expenses
	</button>
</main>