<script>
  import { fahrenheitToCelsius, celsiusToFahrenheit } from "temperature";
  import { getJSON } from "../lib/async.js";
  import { onMount } from "svelte"
  import Weather from "../components/Weather.svelte";

  getJSON("/api/weather?city=charleston,sc&country=us").then(
    console.log.bind(console)
  );

  let temp = 21.7;
  let unit = "c";
  let displayTemp = ''

  $: {
    displayTemp = `${Math.floor(temp)} &deg; ${unit.toUpperCase()}`;
    console.log('displayTemp', displayTemp)
  }

  function convertToF() {
    temp = celsiusToFahrenheit(temp);
    unit = "f";
  }

  function convertToC() {
    temp = fahrenheitToCelsius(temp);
    unit = "c";
  }

  const getWeather = (async () => {
    const results = await getJSON("/api/weather?city=portland,or&country=us");
    return {
      temp: `${results.temp} &deg; C`,
      icon: results.weather.icon,
      description: results.weather.description,
      city: `${results.city_name}, ${results.state_code}`,
    };
  });

</script>
<nav>
  <div>
    <a href="#" on:click|preventDefault="{convertToF}">F</a>
    |
    <a href="#" on:click|preventDefault="{convertToC}">C</a>
  </div>
  <a href="">Favorites</a>
</nav>
<main>
  <div>{@html displayTemp}</div>
  {#await getWeather()} 
    Loading... 
  {:then weather}
  <Weather {...weather} />
  {/await}
</main>
<style>
  nav {
    margin-bottom: 0;
    margin-left: 8px;
    margin-right: 8px;
  }
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }


</style>


