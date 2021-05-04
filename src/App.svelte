<script>
  import { onMount, afterUpdate } from "svelte";
  import Navbar from "./components/Navbar.svelte";
  import ExpenseList from "./components/ExpenseList.svelte";
  import ExpenseData from "./expenses";

  // variables
  let expenses = [];
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

  function setLocalStorageItem(itemName = "expenses", item = []) {
    localStorage.setItem(itemName, JSON.stringify(item));
  }
  function getLocalStorageItem(itemName = "expenses") {
    try {
      return JSON.parse(localStorage.getItem(itemName)) || [...ExpenseData];
    } catch (error) {
      return [];
    }
  }

  onMount(() => {
    expenses = getLocalStorageItem();
  });

  afterUpdate(() => {
    setLocalStorageItem("expenses", expenses);
  });
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
