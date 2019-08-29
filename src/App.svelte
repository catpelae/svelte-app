<script>
  import GasInput from "./GasInput.svelte";
  import PriceList from "./PriceList.svelte";
  import { onMount } from "svelte";

  let gasInput = {
    price: "",
    station: "",
    neighbourhood: "",
    timeStamp: "",
    type: ""
  };
  let gasInputs = [];
  let filteredGasInputs = [];

  onMount(() => {
    if (localStorage.getItem("gasInputs")) {
      gasInputs = JSON.parse(localStorage.getItem("gasInputs"));
      filteredGasInputs = Array.from(gasInputs);
      console.log(gasInputs);
    }
  });

  function storeGasInputs(gasInput) {
    gasInputs = [...gasInputs, gasInput];
    localStorage.setItem("gasInputs", JSON.stringify(gasInputs));
    console.log(localStorage.getItem("gasInputs"));
  }
</script>

<style>
  h1 {
    color: #309b94;
  }
  :global(body) {
    background-color: #f5f5f5;
  }
  .paper {
    margin: 20px;
    background-color: white;
    border: 1px solid #ccc;
    box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.2),
      0px 1px 1px 0px rgba(0, 0, 0, 0.14), 0px 2px 1px -1px rgba(0, 0, 0, 0.12);
    padding: 20px;
  }
</style>

<div class="app">
  <div class="paper">
    <h1>Hello there!</h1>

    <br />
    <GasInput {gasInput} {storeGasInputs} />
  </div>
  <div class="paper priceList">
    <PriceList {gasInputs} {filteredGasInputs} />
  </div>

</div>
