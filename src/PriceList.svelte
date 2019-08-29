<script>
  import { beforeUpdate } from "svelte";
  import filter from "lodash/filter";
  export let gasInputs;
  let filteredGasInputs = [];
  let gasTypeFilter = "all";
  let orderByCol = "price";

  function handleFilterByTypeClick(event) {
    event.preventDefault();
    gasTypeFilter = event.target.name;
    filteredGasInputs = filterByType(gasInputs, gasTypeFilter);
  }
  function filterByType(gasInputs, filterByType) {
    console.log({filterByType})
    if (filterByType === "all") {
      return gasInputs;
    } else {
      return filter(gasInputs, { type: filterByType });
    }
  }
  function handleOrderByCol(event) {
    orderByCol = event.target.name;
    console.log(orderByCol);
  }

  $: {
    // https://svelte.dev/docs#3_$_marks_a_statement_as_reactive
    // the variable to react on needs to be mentioned inside the block
    filteredGasInputs = filterByType(gasInputs, gasTypeFilter);
  }
</script>

<style>
  h2 {
    color: #2b2d2f;
  }
  .table-wrap {
    overflow-x: scroll;
    width: 100%;
  }
  th {
    border-bottom: 3px solid #bebebe;
    text-align: left;
  }
  table {
    width: 100%;
    border-spacing: 0px;
  }
  td {
    min-width: 80px;
    border-bottom: 1px solid #bebebe;
    height: 34px;
    font-size: 14px;
  }

  th,
  td {
    padding: 8px;
  }
  .filter-group {
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
  }
  .filter-group button {
    border-radius: 0px;
    background-color: white;
    border-color: #bebebe;
    color: #309b94;
    font-size: 14px;
  }
  .filter-group button.selected {
    background-color: #309b94;
    color: white;
  }
  button[type="link"] {
    border: none;
    background-color: white;
    color: #309b94;
    font-weight: 400;
    padding: 0px;
    text-decoration: underline;
  }
</style>

<h2>Price List</h2>
{#if gasInputs.length !== 0}
  <div class="filter-group">
    <button
      name="all"
      class={gasTypeFilter === 'all' ? 'selected' : ''}
      on:click={handleFilterByTypeClick}>
      Show all
    </button>
    <button
      name="diesel"
      class={gasTypeFilter === 'diesel' ? 'selected' : ''}
      on:click={handleFilterByTypeClick}>
      Diesel
    </button>
    <button
      name="gas95"
      class={gasTypeFilter === 'gas95' ? 'selected' : ''}
      on:click={handleFilterByTypeClick}>
      Gas 95
    </button>
  </div>
{/if}

{#if filteredGasInputs.length !== 0}
  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>
            <button type="link" name="price" on:click={handleOrderByCol}>
              Price
            </button>
          </th>
          <th>
            <button type="link" name="brand" on:click={handleOrderByCol}>
              Gas station brand
            </button>
          </th>
          <th>
            <button
              type="link"
              name="neighbourhood"
              on:click={handleOrderByCol}>
              Neighbourhood
            </button>
          </th>
          <th>
            <button type="link" name="type" on:click={handleOrderByCol}>
              Type
            </button>
          </th>
          <th>
            <button type="link" name="time" on:click={handleOrderByCol}>
              Time
            </button>
          </th>
        </tr>
      </thead>
      <tbody>
        {#each filteredGasInputs as { price, station, neighbourhood, timeStamp, type }}
          <tr>
            <td>{price}</td>
            <td>{station}</td>
            <td>{neighbourhood}</td>
            <td>{type}</td>
            <td>{timeStamp}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
{:else}
  <p>No data available... yet!</p>
{/if}
