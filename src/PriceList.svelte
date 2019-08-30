<script>
  import { beforeUpdate } from "svelte";
  import filter from "lodash/filter";
  export let gasInputs;
  let gasTypeFilter = "all";
  let orderByCol = "price";
  $: filteredGasInputs = filterByType(gasInputs, gasTypeFilter);

  const handleFilterByTypeClick = (event) => gasTypeFilter = event.target.name;

  const filterByType = (gasInputs, filterByType) => (filterByType === "all") ?  gasInputs: filter(gasInputs, { type: filterByType });

  function handleOrderByCol(event) {
    orderByCol = event.target.name;
    console.log(orderByCol);
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
