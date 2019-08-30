<script>
  import { beforeUpdate } from "svelte";
  import Icon from "fa-svelte";
  import { faAngleDown } from "@fortawesome/free-solid-svg-icons/faAngleDown";
  import { faAngleUp } from "@fortawesome/free-solid-svg-icons/faAngleUp";

  import filter from "lodash/filter";
  import orderBy from "lodash/orderBy";
  export let gasInputs;
  let gasTypeFilter = "all";
  let orderByCol = "price";
  let orderAsc = true;
  $: filteredGasInputs = filterByType(gasInputs, gasTypeFilter);

  const handleFilterByTypeClick = event => (gasTypeFilter = event.target.name);

  const filterByType = (gasInputs, filterByType) =>
    filterByType === "all"
      ? gasInputs
      : filter(gasInputs, { type: filterByType });

  function handleOrderByCol(event) {
    orderByCol = event.target.name;
    orderAsc = !orderAsc;
    let orderByAscDesc = orderAsc ? "asc" : "desc";
    console.log({ orderByCol });
    filteredGasInputs = orderBy(filteredGasInputs, orderByCol, orderByAscDesc);
    console.log({ orderByAscDesc });
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
  th div {
    display: flex;
    flex-direction: row;
    align-items: center;
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
    margin-bottom: 0px;
  }
  :global(.tHeaderIcon) {
    color: #309b94;
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
            <div>
              <button type="link" name="price" on:click={handleOrderByCol}>
                Price
              </button>
              <Icon class="tHeaderIcon" icon={faAngleDown} />
              <Icon class="tHeaderIcon" icon={faAngleUp} />
            </div>

          </th>
          <th>
            <div>
              <button type="link" name="station" on:click={handleOrderByCol}>
                Gas station brand
              </button>
              <Icon class="tHeaderIcon" icon={faAngleDown} />
              <Icon class="tHeaderIcon" icon={faAngleUp} />

            </div>

          </th>
          <th>
            <div>
              <button
                type="link"
                name="neighbourhood"
                on:click={handleOrderByCol}>
                Neighbourhood
              </button>
              <Icon class="tHeaderIcon" icon={faAngleDown} />
              <Icon class="tHeaderIcon" icon={faAngleUp} />

            </div>

          </th>
          <th>
            <div>
              <button type="link" name="type" on:click={handleOrderByCol}>
                Type
              </button>
              <Icon class="tHeaderIcon" icon={faAngleDown} />
              <Icon class="tHeaderIcon" icon={faAngleUp} />

            </div>

          </th>
          <th>
            <div>
              <button type="link" name="timeStamp" on:click={handleOrderByCol}>
                Time
              </button>
              <Icon class="tHeaderIcon" icon={faAngleDown} />
              <Icon class="tHeaderIcon" icon={faAngleUp} />

            </div>

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
