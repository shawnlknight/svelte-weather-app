<script>
  import { blur } from 'svelte/transition'
  import { dispatch } from '../store.js'
  import { router } from 'tinro'
  import CityCard from '../components/CityCard.svelte'
  import page from "page"

  let cities = ['Charleston, SC', 'New York, NY', 'San Francisco, CA']

  function changeCurrentCity(city) {
    return () => {
      router.goto("/")
      dispatch({ type: "SET_CURRENT", payload: city }).then(() => page("/"));
    }
  }
</script>
<div in:blur={{delay: 1000, duration: 1000}}>
  <nav>
    <div>Favorites</div>
    <a href="">Add</a>
  </nav>
  <main>
    <section>
      {#each cities as city}
      <CityCard {city} on:click={changeCurrentCity(city)} />
      {/each}
    </section>
  </main>
</div>
<style>
 nav {
   height: 24px;
   margin-bottom: 0;
   margin-left: 8px;
   margin-right: 8px;
 }
</style>
