<script>
  export let showAddForm;
</script>

<nav class="nav">
  <div class="nav-center">
    <h1 class="nav-title">Budget Calculator</h1>
    <button type="button" class="nav-btn" on:click={showAddForm}>
      <i class="fas fa-plus-square" />
      Add Item
    </button>
  </div>
</nav>
