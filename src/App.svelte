<script>
  import { setContext, onMount, afterUpdate } from "svelte";

  import Navbar from "./components/Navbar.svelte";
  import ExpenseList from "./components/ExpenseList.svelte";
  import Totals from "./components/Totals.svelte";
  import ExpenseForm from "./components/ExpenseForm.svelte";
  import Modal from "./components/Modal.svelte";
  import GithubAwait from "./components/GithubAwait.svelte";
  // import Github from "./components/Github.svelte";

  import expensesData from "./expenses";

  let expenses = [...expensesData];
  let setName = "";
  let setAmount = "";
  let setId = null;
  let isFormOpen = false;

  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = "";
    setId = null;
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function handleSubmit() {
    if (isEditing) {
      editExpense();
    } else {
      addExpense();
    }
    hideForm();
  }

  function addExpense() {
    let expense = {
      id: Math.random() * Date.now(),
      name: setName,
      amount: setAmount
    };
    expenses = [expense, ...expenses];
    setName = "";
    setAmount = "";
    setId = null;
  }
  function editExpense() {
    expenses = expenses.map(item => {
      return item.id === setId
        ? { ...item, name: setName, amount: setAmount }
        : { ...item };
    });
    setName = "";
    setAmount = "";
    setId = null;
  }
  function setModifiedExpense(id) {
    isFormOpen = true;
    let expense = expenses.find(i => i.id === id);
    setId = id;
    setName = expense.name;
    setAmount = expense.amount;
  }

  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);
  setContext("close form", hideForm);

  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
</script>

<style>

</style>

<Navbar {showForm} />
<main class="content">
  <GithubAwait />
  <!-- <Github /> -->
  <!-- <Totals title="total expenses" {total} />
  <ExpenseList {expenses} />
  <button
    class="btn btn-primary btn-block"
    type="button"
    on:click={clearExpenses}>
    clear expenses
  </button> -->
</main>
<!-- {#if isFormOpen}
  <Modal>
    <ExpenseForm
      {handleSubmit}
      bind:name={setName}
      bind:amount={setAmount}
      {isEditing} />
  </Modal>
{/if} -->
