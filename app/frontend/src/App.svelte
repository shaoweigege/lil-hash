<script>
import axios from "axios";
const HOURS = 24;
const SERVER = "https://lilhash.com"
let inputUrl = ""
let hashedUrl = ""
let error = false
$: hashed = hashedUrl.length > 0;
const hash = () => {
  // eslint-disable-next-line
  axios.post(`${SERVER}/hash?url=${encodeURIComponent(inputUrl)}&hours=${HOURS}`)
    .then((response) => {
      const protocol_index = SERVER.indexOf("://");
      hashedUrl = SERVER.slice(protocol_index + 3) + "/" + response.data.key;
      error = false;
    })
    .catch(() => {
      error = true;
    });
}

</script>

{#if !hashed}
<h1 class="text-off-black text-7xl text-black lg:text-8xl font-averia mt-40 dark:text-white">lil' hash</h1>
<p class="text-off-black text-xl text-black lg:text-2xl font-averia dark:text-white">
  simple sharable url shortener
</p>
<label for="url" class="hidden">URL: </label>
<input
  name="url"
  bind:value={inputUrl}
  class="shadow-inner rounded-md text-md w-3/5 md:w-1/2 lg:text-2xl p-2 md:p-3 lg:p-4 m-4 lg:my-10 text-center"
  on:keyup={(e) => e.key==='Enter' && hash()}
/>
<button
  class="px-3 md:px-5 lg:px-8 py-2 md:py-3 lg:py-4 text-md lg:text-2xl rounded-md transition duration-500 bg-white shadow"
  class:bg-red-500={error}
  on:click={hash}
>
  { error ? "try again" : "hash" }
</button>
{:else}
<p class="text-off-black text-xl md:text-lg lg:text-2xl my-4 mt-40 font-averia dark:text-white">
  your link been shortened to
</p>
<h1 class="text-off-black text-4xl md:text-5xl lg:text-6xl xl:text-7xl font-averia dark:text-white">
  { hashedUrl }
</h1>
<p class="text-off-black text-xl md:text-lg lg:text-2xl m-4 md:m-5 lg:m-8 font-averia dark:text-white">
  for the next 24 hours
</p>
{/if}

<style>
.text-off-black {
  color: #35332c;
}
.font-averia {
  font-family: "Averia Serif";
  font-weight: 300;
}
</style>
