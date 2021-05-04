<script>
  export let expenseItem;
  let displayAmount = false;
  export let startEditingExpense;
  export let removeExpense;
</script>

<article id={expenseItem.id} class="single-expense">
  <div class="expense-info">
    <h2>
      {expenseItem.name}
      <button
        class="amount-btn"
        on:click={() => (displayAmount = !displayAmount)}
      >
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4>Amount: ${expenseItem.amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => startEditingExpense(expenseItem)}
    >
      <i class="fas fa-pen" />
    </button>
    <button
      class="expense-btn delete-btn"
      on:click={() => removeExpense(expenseItem)}
    >
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
