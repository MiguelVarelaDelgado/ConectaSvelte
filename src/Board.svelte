<script>
  import { tweened } from "svelte/motion";
  import { cubicOut } from "svelte/easing";
  import Slot from "./Slot.svelte";
  import checkWin from "./checkWin";

  export let onGameEnd;

  let boardNode;
  let board = [
    [0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0]
  ];
  let player = 1;

  const changeBoard = (board, colIndex, newValue) => {
    let isNewBoard = false;
    const newBoard = [...board];
    for (let ri = board.length - 1; ri >= 0; ri -= 1) {
      if (!board[ri][colIndex]) {
        newBoard[ri][colIndex] = newValue;
        isNewBoard = true;
        break;
      }
    }

    return [newBoard, isNewBoard];
  };

  function isBoardFull(board) {
    return !board.some(row => row.includes(0));
  }

  function onPlay(colIndex) {
    console.info(`Player ${player} played at ${colIndex}`);
    
    let isNewBoard;
    [board, isNewBoard] = changeBoard(board, colIndex, player);
    if(!isNewBoard) return;

    const winningPlayer = checkWin(board);
    if (winningPlayer || isBoardFull(board)) {
      onGameEnd(winningPlayer);
    }
    player = player === 1 ? 2 : 1;
  }

  let playingIndex = 0;
  let placeholderPosition = tweened(0, {
    easing: cubicOut
  });

  function handleMousemove(event) {
    const boardNodeX = boardNode.getBoundingClientRect().x;
    const deltaX = event.clientX - boardNodeX - 4;
    playingIndex = Math.min(Math.max(Math.floor(deltaX / (32 + 8)), 0), 6);
    placeholderPosition.set(playingIndex * (32 + 8) + 16);
  }
</script>

<style>
  .container {
    padding: 16px;
    border-radius: 4px;
    background-color: #0963ef;
    position: relative;
  }
  .board {
    cursor: pointer;
    position: relative;
    display: grid;
    grid-template-columns: repeat(7, auto);
  }
  .board-slot {
    padding: 4px;
  }

  .placeholder {
    position: absolute;
    top: -38px;
  }
</style>

<div class="container">
  <div class="placeholder" style="left: {$placeholderPosition}px">
    <Slot value={player} />
  </div>
  <div
    class="board"
    bind:this={boardNode}
    on:mousemove={handleMousemove}
    on:click={() => {
      onPlay(playingIndex);
    }}
  >
    {#each board as row}
      {#each row as value}
        <div class="board-slot">
          <Slot {value} />
        </div>
      {/each}
    {/each}
  </div>
</div>
