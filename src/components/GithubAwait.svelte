<script>
  let url = "https://api.github.com/users";

  async function getUsers() {
    let userData = await fetch(url);
    let githubUsers = await userData.json();
    return githubUsers;
  }
</script>

<section>
  {#await getUsers()}
    <h1>Loading...</h1>
  {:then users}
    {#each users as user, i}
      <article class="user">
        <img src={user.avatar_url} alt={user.login} />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href={user.html_url}>Repo</a>
        </div>
      </article>
    {/each}
  {:catch error}
    <p>Something went wrong: {error.message}</p>
  {/await}
</section>
