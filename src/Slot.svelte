<script>
  const PLAYER_COLORS = ["white", "#F3323A", "#EBDF53"];
  export let value;

  let color;
  $: color = PLAYER_COLORS[value];
</script>

<style>
  .slot {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    border: 1px solid rgb(255, 0, 0);
  }
</style>

<div class="slot" style="background-color: {color}"></div>