<script>
// @ts-nocheck

// @ts-nocheck

  import { count } from './stores.js';
  import Incrementer from './Incrementer.svelte';
  import Decrementer from './Decrementer.svelte';
  import Resetter from './Resetter.svelte';
  import Multiplication from './multiplication.svelte';

  let count_value = 0;
  let generatedTable = '';
  /**
     * @type {Multiplication}
     */
  let multiplicationRef;

  function handleTableGenerated() {
    generatedTable = event.detail.table;
  }

  function useCounterValue() {    
    if (count_value > 0) {
        if (multiplicationRef) {
        multiplicationRef.setRowsAndCols(count_value);
        multiplicationRef.generateTable();
      }
    }
  }

  count.subscribe((value) => {
    count_value = value;
  });
</script>

<style>
  .container {
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background-color: #f0f0f0;
    font-family: 'Arial', sans-serif;
  }
  
  .buttons {
    text-align: center;
    display: flex;
    gap: 10px;
    margin-top: 20px;
  }

  .buttons button {
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    color: #fff;
    background-color: #4caf50;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .buttons button:hover {
    background-color: #45a049;
  }

  #tableContainer {
    margin-top: 20px;
  }

  table {
    margin: auto;
    border-collapse: collapse;
    width: 80%; 
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    font-family: Arial, sans-serif;
  }

  th, td {
    padding: 10px;
    border: 1px solid #ddd;
  }

  th {
    background-color: #4caf50;
    color: white;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  tr:hover {
    background-color: #ddd;
  }

  .count-display {
    margin-top: 20px;
    font-size: 24px;
    font-weight: bold;
  }
</style>

<div class="container">
  <h1>Multiplication Table Generator</h1>
  <Multiplication bind:this={multiplicationRef} on:tableGenerated={handleTableGenerated} />   
  <div class="buttons">
    <Incrementer />
    <Decrementer />
    <Resetter />
    <button on:click={useCounterValue}>Generate Table</button>
  </div>
  <div class="count-display">
    Or Choose Number of Rows and Columns here: {count_value}
  </div>
  <div id="tableContainer">
    {#if generatedTable}
      {@html generatedTable}
    {/if}
  </div>
</div>