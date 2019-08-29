<script>
  export let gasInput;
  export let storeGasInputs;

  let invalidValue = false,
    valueSaved = false;

  let gasStationBrands = [
    { id: 1, name: "UnoX" },
    { id: 2, name: "Shell" },
    { id: 3, name: "Esso" },
    { id: 4, name: "Circle K" }
  ];

  let neighbourhoods = [
    { name: "Bekkestua" },
    { name: "Jar" },
    { name: "Gjønnes" },
    { name: "Sandvika" },
    { name: "Gjettum" },
    { name: "Haslum" },
    { name: "Lysaker" },
    { name: "Strand" }
  ];
  function getCurrentTime() {
    let nowTime = new Date();
    nowTime = nowTime.toString();
    return nowTime.slice(0, nowTime.indexOf("GMT"));
  }
  function handleSave(e) {
    e.preventDefault();
    valueSaved = false;
    invalidValue = false;
    let gasPriceRegex = new RegExp("^\\d{0,2}(\\.\\d{0,2})?$");

    if (gasPriceRegex.test(gasInput.price)) {
      valueSaved = true;
      setTimeout(() => (valueSaved = false), 3000);
      gasInput = {
        price: gasInput.price,
        station: gasInput.station,
        neighbourhood: gasInput.neighbourhood,
        timeStamp: getCurrentTime(),
        type: gasInput.type
      };
      storeGasInputs(gasInput);
    } else {
      console.log("invalid value");
      invalidValue = true;
    }
    gasInput = {
      ...gasInput,
      ...{
        price: "",
        station: "",
        timeStamp: "",
        neighbourhood: "",
        type: ""
      }
    };
  }
</script>

<style>
  form {
    display: flex;
    flex-direction: column;
    max-width: 720px;
  }

  .input-group {
    display: flex;
    flex-direction: row;
    width: 100%;
    justify-content: space-between;
    margin-bottom: 12px;
    margin-top: 8px;
  }
  .input-group.radio {
    justify-content: start;
  }
  .input-group.radio label {
    margin-right: 10px;
    display: flex;
    align-items: center;
  }
  input,
  select {
    margin-top: 8px;
    margin-right: 4px;
    height: 32px;
  }
  button {
    background-color: #309b94;
    width: 180px;
    height: 38px;
    border-radius: 4px;
    color: white;
  }
  .errorMessage {
    color: red;
  }
  .successMessage {
    color: green;
  }

  @media (max-width: 576px) {
    .input-group {
      flex-direction: column;
    }
    .input-group.radio {
      flex-direction: row;
    }
  }
</style>

<form on:submit={handleSave}>
  <div class="">
    <span>Select gas type</span>
    <div class="input-group radio">
      <label>
        <input
          type="radio"
          bind:group={gasInput.type}
          name="type"
          value={'diesel'}
          required />
        Diesel
      </label>
      <label>
        <input
          type="radio"
          bind:group={gasInput.type}
          name="type"
          value={'gas95'}
          required />
        Gas 95
      </label>
    </div>

  </div>

  <div class="input-group">
    <div>
      <label for="currency">Enter gas price</label>
      <input
        bind:value={gasInput.price}
        name="currency"
        placeholder="e.g. 13.00 "
        min="0"
        max="20"
        step=".01"
        type="number"
        required />
    </div>
    <div>
      <label for="location">Enter location</label>
      <select bind:value={gasInput.station} name="location" required>
        <option value="" selected disabled>--Select gas station brand--</option>
        {#each gasStationBrands as station}
          <option value={station.name}>{station.name}</option>
        {/each}
      </select>
      <select bind:value={gasInput.neighbourhood} name="location" required>
        <option value="" selected disabled>--Select neighbourhood--</option>
        {#each neighbourhoods as neighbourhood}
          <option value={neighbourhood.name}>{neighbourhood.name}</option>
        {/each}
      </select>
    </div>
  </div>
  <button type="submit">Save</button>
  {#if valueSaved}
    <p class="successMessage">Value saved!</p>
  {/if}
  {#if invalidValue}
    <p class="errorMessage">Please write a correct value.</p>
  {/if}

</form>
