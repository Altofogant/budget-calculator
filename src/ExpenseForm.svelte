<script>
    export let addExpense;
    import Title from './Title.svelte';
    let name = '';
    let amount = null;
    $: isEmpty = !name || !amount;

    function handleSubmit(e) {
        addExpense({name, amount});
        name = '';
        amount = null;
    }
</script>

<section class="form">
    <Title title="Add expense" />
    <form class="expense-form" on:submit|preventDefault={handleSubmit}>
        <div class="form-control">
            <label for="name">name</label>
            <input type="text" id="name" bind:value={name} />
        </div>
        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="number" bind:value={amount} />
        </div>
        {#if (isEmpty)}
            <p class="form-empty">
                Please fill out all form fields
            </p>
        {/if}
        <button disabled={isEmpty} class:disabled={isEmpty} type="submit" class="btn btn-block">Add expense</button>
        <button type="button" class="close-btn"><i class="fas fa-times"></i>close</button>
    </form>
</section>