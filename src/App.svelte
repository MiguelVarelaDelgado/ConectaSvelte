<script>
	import Board from "./Board.svelte";
  
	let player = -1;
	let gameState = "INICIO";
  
	function handleStartGame() {
	  gameState = "JUGANDO";
	}
  
	function handleGameEnd(winningPlayer) {
	  gameState = "GANO";
	  player = winningPlayer;
	}
  </script>
  
  <style>
	:global(*) {
	  box-sizing: border-box;
	}
	:global(body) {
	  background: #f6fbfd;
	  color: #132a82;
	  margin: 0;
	  padding: 0;
	  font-family: -apple-system, BlinkMacSystemFont, avenir next, avenir,
		helvetica neue, helvetica, Ubuntu, roboto, noto, segoe ui, arial, sans-serif;
	}
	main {
	  width: 100vw;
	  height: 100vh;
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  flex-direction: column;
	}
  
	h1 {
	  font-size: 48px;
	}
  
	button {
	  padding: 8px 16px;
	  font-size: 16px;
	  border-radius: 4px;
	  background-color: #1733aa;
	  border: none;
	  border-bottom: 4px solid #132a82;
	  color: #e7f8fd;
	  cursor: pointer;
	  transition: 200ms background-color, border-color ease-in-out;
	}
  
	button:hover {
	  background-color: #1b43dd;
	  border-color: #1733aa;
	}
	button:focus {
	  background-color: #1b43dd;
	  border-color: #1733aa;
	  outline: 2px solid;
	}
	button:active {
	  outline: none;
	  background-color: #1b43dd;
	  border-color: #1733aa;
	}
  </style>
  
  <main>
	{#if gameState === 'INICIO'}
		<h1>CONECTA 4</h1>
		<button type="button" on:click={handleStartGame}>Jugar</button>
	{:else if gameState === 'JUGANDO'}
	  <Board onGameEnd={handleGameEnd}/>
	{:else}
	  {#if player}
		<h1>JUGADOR {player} GANÓ!</h1>
		<button type="button" on:click={handleStartGame}>Volver a Jugar!</button>
	  {:else}
		<h1>SE LLENÓ, ES UN EMPATE</h1>
		<button type="button" on:click={handleStartGame}>Volver a Jugar!</button>
	  {/if}
	{/if}
  </main>