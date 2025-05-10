<script>
  import { createEventDispatcher } from 'svelte';
  
  /**
     * @type {string | number}
     */
  let rows;
  /**
     * @type {string | number}
     */
  let cols;
  let table = '';
  const dispatch = createEventDispatcher();

  function generateTable() {
    let tempTable = '<table border="8">';    
    tempTable += '<tr><th></th>';
    for (let j = 1; j <= Number(cols); j++) {
      tempTable += `<th>${j}</th>`;
    }
    tempTable += '</tr>';    
    for (let i = 1; i <= Number(rows); i++) {
      tempTable += '<tr>';
      tempTable += `<th>${i}</th>`;      
      for (let j = 1; j <= Number(cols); j++) {
        tempTable += `<td>${i * j}</td>`;
      }
      tempTable += '</tr>';
    }
    tempTable += '</table>';

    table = tempTable;
    dispatch('tableGenerated', { table });

    clearInputs();
  }

  /**
   * @param {number} value
   */
  export function setRowsAndCols(value) {
    rows = value;
    cols = value;
  }
  export function clearInputs() {
    rows = '';
    cols = '';
  }
  export { generateTable };
</script>

<style>
 .container {
    text-align: center;
    margin-top: 20px;
}

input {
    margin: 5px;
}

.tooltip {
    position: relative;
    display: inline-block;
    cursor: pointer;
  }
  
  .tooltip .tooltiptext {
    visibility: hidden;
    width: 140px;
    background-color: black;
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;
    position: absolute;
    z-index: 1;
    bottom: 125%; 
    left: 50%;
    margin-left: -70px; 
    opacity: 0;
    transition: opacity 0.3s;
  }
 
  .tooltip .tooltiptext::after {
    content: "";
    position: absolute;
    top: 100%; 
    left: 50%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: black transparent transparent transparent;
  }
  
  .tooltip:hover .tooltiptext {
    visibility: visible;
    opacity: 1;
  }
</style>

<div class="container">
  <div class="tooltip">
    <label for="rows">Rows:</label>
    <input id="rows" type="number" bind:value={rows} min="1" />
    <span class="tooltiptext">Enter Number For Rows</span>
  </div>
  <div class="tooltip">
    <label for="cols">Cols:</label>
    <input id="cols" type="number" bind:value={cols} min="1" />
    <span class="tooltiptext">Enter Number for Columns</span>
  </div>
  <button on:click={generateTable}>Generate Table</button>
</div>

{#if table}
  {@html table}
{/if}