<script>
  import { onMount, onDestroy } from 'svelte';

  export let targetDate; // e.g., "YYYY-MM-DD"

  let days, hours, minutes, seconds;
  let countdownEnded = false;
  let interval;

  const target = new Date(targetDate + "T00:00:00");

  function updateCountdown() {
    const now = new Date().getTime();
    const distance = target.getTime() - now;

    if (distance < 0) {
      countdownEnded = true;
      clearInterval(interval);
      return;
    }

    days = Math.floor(distance / (1000 * 60 * 60 * 24));
    hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
    seconds = Math.floor((distance % (1000 * 60)) / 1000);
  }

  onMount(() => {
    updateCountdown(); // Run once immediately
    interval = setInterval(updateCountdown, 1000);
  });

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

<style>
  .countdown-container {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin-top: 1rem;
  }
  .time-block {
    text-align: center;
  }
  .time-value {
    font-size: 1.5rem;
    font-weight: 600;
  }
  .time-label {
    font-size: 0.8rem;
    text-transform: uppercase;
    color: #6E8483; /* Using the hover color for a muted feel */
  }
  .end-message {
    font-size: 1.5rem;
  }
</style>

<div class="countdown-container">
  {#if countdownEnded}
    <div class="end-message">The day is here!</div>
  {:else}
    <div class="time-block">
      <div class="time-value">{days}</div>
      <div class="time-label">Days</div>
    </div>
    <div class="time-block">
      <div class="time-value">{hours}</div>
      <div class="time-label">Hours</div>
    </div>
    <div class="time-block">
      <div class="time-value">{minutes}</div>
      <div class="time-label">Minutes</div>
    </div>
    <div class="time-block">
      <div class="time-value">{seconds}</div>
      <div class="time-label">Seconds</div>
    </div>
  {/if}
</div> 