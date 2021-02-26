<script>
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
</script>

<section>
  <SectionTitle title="expense list" />
  <ul>
    {#each expenses as expense, i (expense.id)}
      <div
        in:fly={{ x: 200, delay: (i + 1) * 700 }}
        out:fly={{ x: 200 }}
        animate:flip>
        <Expense {...expense} />
      </div>
    {:else}
      <h2>Currently there are no expenses</h2>
    {/each}
  </ul>
</section>
