<script>
	import Modal from './Modal.svelte';
	import Navbar from './Navbar.svelte';
	//import ExpensesData from './expenses.js';
	import ExpensesList from './ExpensesList.svelte';
	import { setContext, onMount, afterUpdate } from 'svelte';
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
	}

	function clearExpenses() {
		expenses = [];
	}

	function addExpense({name, amount}) {
		let expense = {id: Math.random() * Date.now(), name, amount};
		expenses = [expense,...expenses];
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
	}

	setContext('remove', removeExpense);
	setContext('modify', setModifiedExpense);

	function setLocalStorage() {
		localStorage.setItem('expenses', JSON.stringify(expenses));
	}

	onMount(() => {
		expenses = localStorage.getItem('expenses') ? JSON.parse(localStorage.getItem('expenses')) : [];
	});
	
	afterUpdate(() => {
		setLocalStorage();
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

<Modal>
	<h1 slot="header">Hello world</h1>
	<p slot="footer">Footer slots</p>
</Modal>