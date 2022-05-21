<script>
	import Navbar from './Navbar.svelte';
	import ExpensesData from './expenses.js';
	import ExpensesList from './ExpensesList.svelte';

	let expenses = [...ExpensesData];
</script>

<Navbar />
<main class="content">
	<ExpensesList expenses={expenses} />
</main>