<script>
  import guestsData from '../data/guests.json'; // Import guests from JSON

  let guestName = '';
  let foundTable = null;
  let errorMessage = '';
  let highlightedTableId = '';
  let confirmedGuestName = '';

  // Use the imported guests data
  const guests = guestsData;

  function findTable() {
    foundTable = null;
    errorMessage = '';
    highlightedTableId = '';
    confirmedGuestName = '';

    const searchTerm = guestName.trim().toLowerCase();
    if (!searchTerm) {
      errorMessage = 'Please enter a name.';
      return;
    }

    const foundGuest = guests.find(guest => guest.name.toLowerCase().includes(searchTerm));

    if (foundGuest) {
      foundTable = foundGuest.table;
      highlightedTableId = `table-${foundGuest.table}`;
      confirmedGuestName = foundGuest.name;
    } else {
      errorMessage = 'Guest not found. Please check the name or ask for assistance.';
    }
  }
</script>

<div class="table-finder-container">
  <h2>Find Your Table</h2>
  <div class="input-area">
    <label for="guestName">Enter your full name:</label>
    <input type="text" id="guestName" bind:value={guestName} placeholder="e.g., Alice Wonderland" />
    <button on:click={findTable}>Find My Table</button>
  </div>

  {#if foundTable !== null && confirmedGuestName}
    <p class="found-message">Welcome, {confirmedGuestName}! You are at <strong>Table {foundTable}</strong>.</p>
  {:else if errorMessage}
    <p class="error-message">{errorMessage}</p>
  {/if}

  <div class="seating-map">
    <svg viewBox="0 0 400 360" preserveAspectRatio="xMidYMid meet" aria-labelledby="mapTitle mapDesc">
      <title id="mapTitle">Seating Chart</title>
      <desc id="mapDesc">A diagram of a room with a front door and rear door. It contains 8 round tables arranged in four rows and two columns. The table for the searched guest will be highlighted.</desc>

      <!-- Room Elements -->
      <rect x="150" y="10" width="100" height="15" class="room-feature" />
      <text x="200" y="22" text-anchor="middle" class="room-label">Front Door</text>
      
      <!-- Table representations: 4 rows, 2 columns -->
      <!-- Row 1 -->
      <circle id="table-1" cx="120" cy="60" r="30" class:highlighted={highlightedTableId === 'table-1'} />
      <text x="120" y="65" text-anchor="middle" class="table-label">1</text>
      <circle id="table-2" cx="280" cy="60" r="30" class:highlighted={highlightedTableId === 'table-2'} />
      <text x="280" y="65" text-anchor="middle" class="table-label">2</text>

      <!-- Row 2 -->
      <circle id="table-3" cx="120" cy="130" r="30" class:highlighted={highlightedTableId === 'table-3'} />
      <text x="120" y="135" text-anchor="middle" class="table-label">3</text>
      <circle id="table-4" cx="280" cy="130" r="30" class:highlighted={highlightedTableId === 'table-4'} />
      <text x="280" y="135" text-anchor="middle" class="table-label">4</text>

      <!-- Row 3 -->
      <circle id="table-5" cx="120" cy="200" r="30" class:highlighted={highlightedTableId === 'table-5'} />
      <text x="120" y="205" text-anchor="middle" class="table-label">5</text>
      <circle id="table-6" cx="280" cy="200" r="30" class:highlighted={highlightedTableId === 'table-6'} />
      <text x="280" y="205" text-anchor="middle" class="table-label">6</text>

      <!-- Row 4 -->
      <circle id="table-7" cx="120" cy="270" r="30" class:highlighted={highlightedTableId === 'table-7'} />
      <text x="120" y="275" text-anchor="middle" class="table-label">7</text>
      <circle id="table-8" cx="280" cy="270" r="30" class:highlighted={highlightedTableId === 'table-8'} />
      <text x="280" y="275" text-anchor="middle" class="table-label">8</text>

      <!-- Head Table Removed -->

      <rect x="150" y="340" width="100" height="15" class="room-feature" />
      <text x="200" y="352" text-anchor="middle" class="room-label">Rear Door</text>

    </svg>
  </div>
</div>

<style>
  .table-finder-container {
    font-family: sans-serif;
    max-width: 600px;
    margin: 2rem auto;
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
  }
  .input-area {
    display: flex;
    gap: 0.5rem;
    align-items: center;
    margin-bottom: 1rem;
  }
  .input-area label {
    font-weight: bold;
  }
  .input-area input {
    flex-grow: 1;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .input-area button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .input-area button:hover {
    background-color: #0056b3;
  }
  .error-message {
    color: red;
    font-weight: bold;
  }
  .found-message {
    color: green;
    font-size: 1.2rem;
    margin-bottom: 1rem;
  }
  .seating-map {
    margin-top: 1.5rem;
    border: 1px solid #ccc;
    padding: 0.5rem;
    background-color: white;
  }
  .seating-map svg {
    width: 100%;
    height: auto;
  }
  .seating-map circle {
    fill: #b0e0e6; /* Powder blue for tables */
    stroke: #555;
    stroke-width: 1;
    transition: fill 0.3s ease;
  }
  .seating-map circle.highlighted {
    fill: #ffcc00; /* Yellow highlight */
    stroke: #E69B00;
    stroke-width: 2;
  }
  .table-label {
    font-size: 12px;
    fill: #333;
    pointer-events: none; /* So they don't interfere with clicks on circles if any */
  }
  .room-feature {
    fill: #ddd;
    stroke: #bbb;
  }
  .room-label {
    font-size: 10px;
    fill: #555;
    text-anchor: middle;
  }
</style> 