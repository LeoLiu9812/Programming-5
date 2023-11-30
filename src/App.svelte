<script>
  import { Router, Link, Route } from "svelte-routing";
  import { writable } from 'svelte/store';

  let heroes = [
    { name: "Tracer", description: "Tracer is an energetic and agile adventurer who can manipulate time.", image: "tracer.jpg" },
    { name: "Reinhardt", description: "Reinhardt is a powerful tank with a massive rocket hammer and a strong sense of justice.", image: "reinhardt.jpg" },
    { name: "Widowmaker", description: "Widowmaker is a deadly sniper with a knack for finding the perfect shot.", image: "widowmaker.jpg" }
  ];

  let selectedHero = writable(null);

  function toggleHero(hero) {
    console.log("Toggling hero:", hero);
    console.log("Current selectedHero:", $selectedHero);
    selectedHero.update(current => current === hero ? null : hero);
  }
</script>

<main>
  <h1>Overwatch Heroes Introduction</h1>

  {#each heroes as hero}
    <Router>
      <nav>
        <Link to={`/${hero.name.toLowerCase()}`}>
          <a class:selected={$selectedHero && $selectedHero.name === hero.name}>{hero.name}</a>
        </Link>
      </nav>

      <Route path={`/${hero.name.toLowerCase()}`}>
        <div class="hero-container">
          <h2>{hero.name}</h2>
          <img src={`images/${hero.image}`} alt={hero.name} style="max-width: 100%; border-radius: 10px; margin-bottom: 20px;">
          <button on:click={() => toggleHero(hero)}>
            {#if $selectedHero && $selectedHero.name === hero.name}
              Close Description
            {:else}
              View Description
            {/if}
          </button>
          {#if $selectedHero && $selectedHero.name === hero.name}
            <p>{$selectedHero.description}</p>
          {/if}
          {#if !$selectedHero || ($selectedHero && $selectedHero.name !== hero.name)}
            <p>No description available.</p>
          {/if}
        </div>
      </Route>
    </Router>
  {/each}
</main>


<style>
  main {
    text-align: center;
    padding: 40px;
    max-width: 800px;
    margin: 0 auto;
    background-color: #f9f9f9;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  }

  h1 {
    color: #333;
    text-transform: uppercase;
    font-size: 32px;
    font-weight: 700;
    margin-bottom: 30px;
  }

  nav {
    display: flex;
    justify-content: space-around;
    margin-bottom: 30px;
  }

  a {
    text-decoration: none;
    padding: 15px 30px;
    color: #fff;
    background-color: #3498db;
    border-radius: 8px;
    transition: background-color 0.3s ease;
  }

  a:hover {
    background-color: #2980b9;
  }

  button {
    margin-top: 30px;
    padding: 15px 30px;
    background-color: #e74c3c;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #c0392b;
  }

  .hero-container {
    background-color: #fff;
    padding: 30px;
    border-radius: 15px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }

  p {
    margin-top: 20px;
    font-size: 18px;
    color: #555;
  }
</style>
