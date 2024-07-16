
<script>
  let fineAmount = 0;
  let daysInPrison = 0;
  let conversionRate = 50;

  $: calculatedDays = Math.floor(fineAmount / conversionRate);

  function handleCalculation() {
    daysInPrison = calculatedDays;
  }
</script>

<main>
  <h1>Sakonmuuntolaskuri</h1>
  
  <div>
    <label for="fineAmount">Päiväsakkojen määrä:</label>
    <input id="fineAmount" type="number" bind:value={fineAmount}>
  </div>

  <div>
    <label for="conversionRate">Uhkasakkojen määrä:</label>
    <input id="conversionRate" type="number" bind:value={conversionRate}>
  </div>

  <div>
    <label for="daysInPrison">Days in Prison:</label>
    <input id="daysInPrison" type="number" bind:value={daysInPrison}>
  </div>

  <button on:click={handleCalculation}>Calculate</button>

  <p>Calculated Days: {calculatedDays}</p>
</main>

<style>
  main {
    padding: 1em;
    max-width: 300px;
    margin: 0 auto;
  }
  div {
    margin-bottom: 1em;
  }
  label {
    display: block;
    margin-bottom: 0.5em;
  }
  input {
    width: 100%;
    padding: 0.5em;
  }
  button {
    width: 100%;
    padding: 0.5em;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
  }
  button:hover {
    background-color: #45a049;
  }
</style>