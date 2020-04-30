<template>
  <div class="game-board">
    <div class="game-title">
      <span class="label-letter">z</span>
      <span class="label-letter">o</span>
      <span class="label-letter">o</span>
      <span class="label-letter">m</span>
    </div>
    <div class="game-squares">
      <game-square
        v-for="(item, idx) in randomPhrases"
        :key="idx"
        :id="idx"
        :text="item"
        :status="squareStatus[idx]"
        @toggle-state="toggleState"
      />
    </div>
    <div class="game-title">
      <span class="label-letter">b</span>
      <span class="label-letter">i</span>
      <span class="label-letter">n</span>
      <span class="label-letter">g</span>
      <span class="label-letter">o</span>
    </div>
  </div>
</template>

<script>
import GameSquare from './GameSquare';
const FREE_SPACE_INDEX = 12;
const bingos = [
  [0, 1, 2, 3, 4],
  [5, 6, 7, 8, 9],
  [10, 11, 12, 13, 14],
  [15, 16, 17, 18, 19],
  [20, 21, 22, 23, 24],
  [0, 5, 10, 15, 20],
  [1, 6, 11, 16, 21],
  [2, 7, 12, 17, 22],
  [3, 8, 13, 18, 23],
  [4, 9, 14, 19, 24],
  [0, 6, 12, 18, 24],
  [4, 8, 12, 16, 20]
];
export default {
  components: {
    GameSquare
  },
  props: {
    phrases: {
      type: Array
    }
  },
  data() {
    return {
      squareStatus: new Array(25).fill(0)
    };
  },
  computed: {
    randomPhrases() {
      let copy = [...this.phrases];
      // shuffle the array
      copy.sort(() => Math.random() - 0.5);
      copy[FREE_SPACE_INDEX] = `FREE SPACE`;
      return copy;
    }
  },
  methods: {
    toggleState(idx) {
      const newState = this.squareStatus[idx] ? 0 : 1;
      this.$set(this.squareStatus, idx, newState);
      this.checkForBingo();
    },
    checkForBingo() {
      // Figure out which squares are part of a Bingo
      const winningSquares = new Set();
      bingos.forEach(set => {
        let bingo = true;
        let setIdx = 0;
        while (bingo & (setIdx < set.length)) {
          bingo = bingo && this.squareStatus[set[setIdx++]] > 0;
        }
        if (bingo) {
          set.forEach(el => winningSquares.add(el));
        }
      });

      // Set the status for each square
      for (let i = 0; i < this.squareStatus.length; i++) {
        if (winningSquares.has(i)) {
          this.$set(this.squareStatus, i, 2);
        } else if (this.squareStatus[i] === 2) {
          // This is no longer a bingo square
          this.$set(this.squareStatus, i, 1);
        }
      }
    }
  },
  mounted() {
    // pre-select the free space square
    this.$set(this.squareStatus, FREE_SPACE_INDEX, 1);
  }
};
</script>

<style scoped>
.game-board {
  background-color: darkgrey;
}

.game-squares {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1.5rem;
  margin: 1rem;
}

.game-title {
  display: flex;
  justify-content: space-around;
}

.label-letter {
  font-size: 3rem;
  font-weight: bold;
  color: var(--zoom-color);
  text-align: center;
}
</style>
