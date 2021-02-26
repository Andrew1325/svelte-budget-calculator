<script>
  import { getContext } from "svelte";
  // import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";

  // onMount(() => {
  //   console.log("form has mounted");
  // });
  // beforeUpdate(() => {
  //   console.log("before update");
  // });
  // afterUpdate(() => {
  //   console.log("after update");
  // });
  // onDestroy(() => {
  //   console.log("on destroy");
  // });
  import Title from "./Title.svelte";

  export let name = "";
  export let amount = "";
  export let isEditing;
  export let handleSubmit;
  // export let hideForm;

  const hideForm = getContext("close form");

  $: empty = !name || !amount || typeof amount !== "number";
</script>

<section class="form">
  <Title title={isEditing ? 'edit expense' : 'add expense'} />
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <label for="name">name</label>
      <input id="name" type="text" bind:value={name} />
    </div>
    <div class="form-control">
      <label for="amount">amount</label>
      <input id="amount" type="number" bind:value={amount} />
    </div>
    {#if empty}
      <p class="form-empty">please fill out all form fields</p>
    {/if}
    <button
      disabled={empty}
      class:disabled={empty}
      class="btn btn-block"
      type="submit">
      {#if isEditing}update expense{:else}add expense{/if}

    </button>
    <button
      class="close-btn"
      type="button"
      on:click={() => {
        hideForm();
      }}>
      <i class="fas fa-times" />
      close
    </button>
  </form>
</section>
