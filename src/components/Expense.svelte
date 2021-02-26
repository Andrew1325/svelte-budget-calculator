<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { getContext } from "svelte";

  export let id;
  export let name;
  export let amount;

  let displayAmount = false;

  const removeExpense = getContext("remove");
  const setModifiedExpense = getContext("modify");
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button
        class="amount-btn"
        on:click={() => {
          displayAmount = !displayAmount;
        }}>
        {#if !displayAmount}
          <i class="fas fa-caret-down" />
        {:else}
          <i class="fas fa-caret-up" />
        {/if}

      </button>
    </h2>
    {#if displayAmount}
      <h4 in:fly={{ x: -100, y: 100, duration: 900, delay: 200 }} out:slide>
        amount: ${amount}
      </h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
