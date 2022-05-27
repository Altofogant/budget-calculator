<script>
    // import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte';
    // onMount(() => {
    //     console.log("form has mounted");
    // });
    // beforeUpdate(() => {
    //     console.count("before update");
    // });
    // afterUpdate(() => {
    //     console.count("after update");
    // });
    // onDestroy(() => {
    //     console.log("form is hidden");
    // });
    
    export let addExpense;
    import Title from './Title.svelte';
    export let name = '';
    export let amount = null;
    export let isEditing;
    export let editExpense;
    export let hideForm;
    $: isEmpty = !name || !amount;

    function handleSubmit(e) {
        if (isEditing) {
            editExpense({name, amount});
        } else {
            addExpense({amount, name});
        }
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
        <button disabled={isEmpty} class:disabled={isEmpty} type="submit" class="btn btn-block">{#if isEditing} Edit expense {:else} Add expense {/if}</button>
        <button type="button" class="close-btn" on:click={hideForm}><i class="fas fa-times"></i>close</button>
    </form>
</section>