<script>
  import "@fontsource/roboto";
  import {
    currentPageNumber,
    randomNumber,
    trialTimes,
  } from "../store/pageSteps";
  import { onMount } from "svelte";
  import { VideosURLs, WatchedVideos, FilteredVideos } from "../store/index";
  
  import Error from "../components/Error.svelte";
  import TrialFinishedError from "../components/TrialFinishedError.svelte";

  //   variables for different actions and purpose
  let error = false;
  let inputValue1 = "";
  let inputValue2 = "";
  let randomValue = "";
  let trial_Times = 0;
  // checking user input that whether it is same with previous displayed value or not
  const checkingUserInput = () => {
    // getting previous display value from store
    randomNumber.subscribe((value) => {
      randomValue = value;
    });
  };

  // Triggered next page if everything is working correctly
  const NextPageHandler = () => {
    currentPageNumber.set(4);
  };

  // Handle the click event
  const clickHandler = () => {
    // Conditional statement for checking user input
    if (randomValue == inputValue1 && inputValue2 == "") {
      NextPageHandler();
    } else {
      error = true;
      trialTimes.set(++trial_Times);
    }
  };
  // increment trial times
  const IncrementTrialTimes = () => {
    trialTimes.subscribe((value) => {
      trial_Times = value;
    });
  };
  
// ---------- Filtering videosUrls -------- 
const  FilteringVideosURLs = () =>{
// Extract the last part of each URL in arr1
    let arr1LastParts = $VideosURLs.map((url) => url.split("/").pop());
    
    let updatedArr = $VideosURLs.filter(
      (item) => !$WatchedVideos.includes(item.split("/").pop())
    );
    FilteredVideos.set(updatedArr);
}

  // onMount which is called whenever component is rendered
  onMount(() => {
    IncrementTrialTimes();
    checkingUserInput();
    FilteringVideosURLs();
  });
</script>

<!-- JSX Section -->
{#if error}
  {#if trial_Times > 3}
    <TrialFinishedError />
  {:else}
    <Error />
  {/if}
{:else}
  <div
    class="container w-full h-screen flex justify-center items-center overflow-hidden"
  >
    <div
      class="wrapper mx-4 flex flex-col gap-7 justify-center items-center text-xl text-gray-700"
    >
      <h1>Textbox</h1>

      <input
        type="text"
        class="w-[11rem] text-sm p-2 h-6 border border-gray-900 rounded-sm"
        bind:value={inputValue1}
      />

      <input
        type="text"
        class="TextBOX2 hidden w-[11rem] text-sm p-2 h-6 border border-gray-900 rounded-sm"
        bind:value={inputValue2}
      />

      <button
        on:click={clickHandler}
        class="py-1 border border-gray-400 text-sm rounded-md px-3 hover:bg-gray-200"
        >Continue
      </button>
    </div>
  </div>
{/if}
