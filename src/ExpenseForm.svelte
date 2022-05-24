<script>
    import Title from './Title.svelte';
</script>

<section class="form">
    <Title title="Add expense" />
    <form class="expense-form">
        <div class="form-control">
            <label for="name">name</label>
            <input type="text" id="name" />
        </div>
        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="number" />
        </div>
        <p class="form-empty">
            Please fill out all form fields
        </p>
        <button type="submit" class="btn btn-block">Add expense</button>
        <button type="button" class="close-btn"><i class="fas fa-times"></i>close</button>
    </form>
</section>