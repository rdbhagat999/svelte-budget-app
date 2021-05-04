<script>
  import Navbar from "./components/Navbar.svelte";
  import ExpenseList from "./components/ExpenseList.svelte";
  import ExpenseData from "./expenses";

  // variables
  let expenses = [...ExpenseData];
  let isFormDisplayed = false;

  function showAddForm() {
    isFormDisplayed = true;
  }
  function hideAddForm() {
    isFormDisplayed = false;
  }
  function toggleForm() {
    isFormDisplayed = !isFormDisplayed;
  }
</script>

<Navbar {showAddForm} />

<main class="content">
  <ExpenseList
    bind:expenses
    {isFormDisplayed}
    {showAddForm}
    {hideAddForm}
    {toggleForm}
  />
</main>
