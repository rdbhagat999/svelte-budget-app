<script>
  import { fly, fade } from "svelte/transition";
  import { flip } from "svelte/animate";
  import AddExpense from "./AddExpense.svelte";
  import TotalExpense from "./TotalExpense.svelte";
  import ExpenseItem from "./ExpenseItem.svelte";
  import SectionTitle from "./Title.svelte";
  import Modal from "./Modal.svelte";

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
    <div transition:fade>
      <Modal>
        <AddExpense bind:expense {toggleForm} {addExpense} {updateExpense} />
      </Modal>
    </div>
  {/if}

  <SectionTitle title="Expense List" />

  <ul>
    {#each expenses as expenseItem, index (expenseItem.id)}
      <li
        in:fly={{ x: -200, duration: (index + 1) * 750 }}
        out:fly={{ x: -200, duration: 550 }}
        animate:flip
      >
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
