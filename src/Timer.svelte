<script>
  // imports
  import { createEventDispatcher } from 'svelte';
  import ProgressBar from './ProgressBar.svelte';
  const totalSeconds = 20;
  // variables
  let secondsLeft = totalSeconds;
  let isRunning = false;

  // dispatch event for play sound
  const dispatch = createEventDispatcher();

  // functions
  function startTimer() {
    isRunning = true;
    const timer = setInterval(() => {
      secondsLeft -= 1;
      if (secondsLeft == 0) {
        clearInterval(timer);
        isRunning = false;
        secondsLeft = totalSeconds;
        dispatch('end');
      }
    }, 1000);
  }

  // Progress %
  $: progress = ((totalSeconds - secondsLeft) / totalSeconds) * 100;
</script>

<div bp="grid">
  <h2 bp="offset-5@md 4@md 12@sm">Seconds Left: {secondsLeft}</h2>
</div>

<ProgressBar {progress} />
<div bp="grid">
  <button
    disabled={isRunning}
    bp="offset-5@md 4@md 12@sm"
    class="start"
    on:click={startTimer}
  >
    Start
  </button>
</div>

<style>
  h2 {
    margin: 0;
  }
  .start {
    background-color: rgb(154, 73, 73);
    margin: 10px 0;
    width: 100%;
    transition: all 0.3s ease-in-out;
  }
  .start:active {
    transform: scale(1.1);
  }
  .start[disabled] {
    background-color: rgb(194, 194, 194);
    cursor: not-allowed;
  }
</style>
