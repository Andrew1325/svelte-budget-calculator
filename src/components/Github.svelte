<script>
  import { onMount } from "svelte";

  let url = "https://api.github.com/users";
  let users = [];
  let loading = true;
  onMount(async () => {
    let userData = await fetch(url);
    let githubUsers = await userData.json();
    users = githubUsers;
    loading = false;
  });
</script>

<style>
  h2 {
    text-align: center;
  }
  img {
    border-radius: 50%;
  }
</style>

{#if loading}
  <h2>loading...</h2>
{:else}
  <section>
    {#each users as user, i}
      <article class="user">
        <img src={user.avatar_url} alt={user.login} />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href={user.html_url}>Repo</a>
        </div>
      </article>
    {/each}
  </section>
{/if}
