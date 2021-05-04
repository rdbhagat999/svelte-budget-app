<script>
  import SectionTitle from "./Title.svelte";

  export let expense = {};
  export let isEditing = false;
  export let addExpense;
  export let updateExpense;
  export let toggleForm;

  $: isValid = expense.name && expense.name.trim() && expense.amount > 0;
  $: isEditing = expense.id > 0;

  function resetForm() {
    expense = {};
    isEditing = false;
  }

  function handleSubmit(expense) {
    if (isValid) {
      if (isEditing) {
        updateExpense({ ...expense });
      } else {
        addExpense({ ...expense });
      }
      resetForm();
    }
    return;
  }
</script>

<section class="form">
  <SectionTitle title="add expense" />

  <form
    class="expense-form"
    on:submit|preventDefault={() => handleSubmit(expense)}
  >
    <div class="form-control">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={expense.name} />
    </div>
    <div class="form-control">
      <label for="amount">Amount</label>
      <input type="number" id="amount" bind:value={expense.amount} />
    </div>
    {#if !isValid}
      <p class="form-empty">All fields are required</p>
    {/if}
    <button
      type="submit"
      class="btn btn-block"
      class:disabled={!isValid}
      disabled={!isValid}
    >
      {isEditing ? "Update Expense" : "Add expense"}</button
    >
    <button
      type="button"
      class="close-btn"
      on:click|stopPropagation={() => toggleForm()}
    >
      <i class="fas fa-times" />
      Close
    </button>
  </form>
</section>
