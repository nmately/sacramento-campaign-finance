<script>
  import { onMount } from "svelte";
  import { writable } from "svelte/store";
  import Race from "$lib/Race.svelte";
  export let data = {}
  
  $: dateGeneratedAt = data.dateGeneratedAt
  $: races = data.races
  
  const isTocExpanded = writable(true);
  let isJavascriptEnabledInBrowser = false;

  onMount(() => {
    isJavascriptEnabledInBrowser = true;
  });
</script>

<svelte:head>
  <title>Following the money in Sacramento elections</title>

  <style>
    body {
      background-color: #3d521e;
      color: white;
    }
  </style>
</svelte:head>

<section>
  <div class="intro well-width">
    <h1>Sacramento campaign cash</h1>
    <p>
      This site helps answer the question: who is funding each candidate or
      measure?
    </p>
    <p>
      Both the city and county have websites that purport to allow the public
      access to campaign finance information, but they're hard to use and
      clunky. Below, you can see the individual contributors to each campaign as
      well as the groups that are spending their own money on advertisements, called
      "independent expenditures."
    </p>
    <p class="last-updated">
      The last time we checked for new data was {dateGeneratedAt}.
    </p>
  </div>

  <div class="races-toc well-width">
    {#if $isTocExpanded}
      <strong>Races</strong>
      <ul>
        {#each races as race}
          <li class="race">
            <a href={`#${race.race}`}>{race.race}</a>
          </li>
        {/each}
      </ul>
    {/if}
    {#if isJavascriptEnabledInBrowser}
      <button
        on:click={() => {
          $isTocExpanded = !$isTocExpanded;
        }}
      >
        {#if $isTocExpanded}Close{:else}Show races{/if}
      </button>
    {/if}
  </div>

  <ul class="races">
    {#each races as race}
      <li class="race">
        <Race data={race} />
      </li>
    {/each}
  </ul>
</section>

<footer>
  <p>
    Made by <a href="https://github.com/jeremiak/sacramento-campaign-finance"
      >Jeremia</a
    > in 2022 because it should be easier to know who contributes to local politicians.
  </p>
  <p>
    Want to use the data for something? You can download <a href="/data.csv"
      >it here</a
    >.
  </p>
  <p>
    Looking for this page as it looked at the end of the primary? That's <a
      href="https://62a13b239387a00008d3999a--sacramento-campaign-cash.netlify.app/"
      >right here</a
    >.
  </p>
</footer>

<style lang="scss">
  .well-width {
    color: white;
    max-width: 750px;
    margin: 0 auto;
  }

  .intro {
    background-color: white;
    border: 1.5px solid #3d521e;
    border-bottom: none;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    color: #062726;
    padding: 1rem;
    font-size: 1em;
    
    .last-updated {
      margin-bottom: 0;
    }
  }
  h1 {
    font-weight: 700;
    text-align: center;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  .races-toc {
    background-color: #ffffff;
    border: 1.5px solid #3d521e;
    border-top: none;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    color: #062726;
    font-size: 1em;
    margin: 0 auto;
    padding: 1.5rem;
    padding-bottom: 0.75rem;
    position: sticky;
    text-align: center;
    top: 0;

    ul {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }

    li {
      margin-bottom: 0.5rem;
      margin-right: 0.75rem;
    }

    a {
      color: inherit;
      // text-decoration: none;
    }

    a:after {
      content: "•";
      display: inline-block;
      margin-left: 0.75rem;
    }

    li:last-child a:after {
      display: none;
    }
  }

  footer {
    color: white;
    font-size: 0.9rem;
    padding-bottom: 1rem;
    text-align: center;

    a {
      color: white;
    }

    marquee span {
      margin-right: 30vw;
    }
  }
</style>
