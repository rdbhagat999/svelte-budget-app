<script>
  import AddExpense from "./AddExpense.svelte";
  import TotalExpense from "./TotalExpense.svelte";
  import ExpenseItem from "./ExpenseItem.svelte";
  import SectionTitle from "./Title.svelte";

  export let expenses = [];
  export let toggleForm;
  export let showAddForm;
  export let hideAddForm;
  export let isFormDisplayed = false;
  let expense = {};

  // reactive
  $: expenseTotal = expenses.reduce((sum, exp) => (sum += exp.amount), 0);
  $: lastExpenseId = expenses.length
    ? Math.max(...expenses.map((t) => t.id)) + 1
    : 1;

  // functions

  function addExpense(expense) {
    expense.id = lastExpenseId;
    expenses = [expense, ...expenses];
  }

  function startEditingExpense(editExpense) {
    expense = editExpense;
    showAddForm();
  }

  function updateExpense(updatedExpense) {
    expenses = expenses.map((item) => {
      if (item.id === updatedExpense.id) {
        item = { ...item, ...updatedExpense };
      }
      return { ...item };
    });
    stopEditing();
  }

  function stopEditing() {
    expense = {};
    hideAddForm();
  }

  function removeExpense(expense) {
    expenses = expenses.filter((exp) => exp.id !== expense.id);
  }
  function clearExpenses() {
    expenses = [];
    hideAddForm();
  }
</script>

<section>
  <TotalExpense title="Total Expenses" {expenseTotal} />

  {#if isFormDisplayed}
    <AddExpense bind:expense {toggleForm} {addExpense} {updateExpense} />
  {/if}

  <SectionTitle title="Expense List" />

  <ul>
    {#each expenses as expenseItem (expenseItem.id)}
      <li>
        <ExpenseItem {expenseItem} {startEditingExpense} {removeExpense} />
      </li>
    {:else}
      <li>
        <h2>Nothing to display here</h2>
      </li>
    {/each}
  </ul>
</section>

<button class="btn btn-primary btn-block" on:click={clearExpenses}>
  clear Expenses
</button>

<style>
  h2 {
    text-transform: capitalize;
    font-size: 1rem;
  }
</style>
