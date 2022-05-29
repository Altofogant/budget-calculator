<script>
    import SectionTitle from './Title.svelte';
    import Expense from './Expense.svelte';
    import { fly } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    export let expenses = [];
    export let removeExpense;
</script>

<style>
    h2 {
        text-transform: capitalize;
    }
</style>

<section>
    <SectionTitle title="expense list" />
    <ul>
        {#each expenses as expense, index (expense.id)}
        <div transition:fly={{ x:200, delay:index*50 }} animate:flip="{{duration: 200}}">
            <Expense {...expense} />
        </div>
        {:else}
            <h2>No expenses added to the list</h2>
        {/each}
    </ul>
</section>