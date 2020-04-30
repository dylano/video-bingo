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
      squareStatus: new Array(25).fill(false)
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
      const newState = !this.squareStatus[idx];
      this.$set(this.squareStatus, idx, newState);
      this.checkForBingo(idx);
    },
    checkForBingo(index) {
      const possibleBingos = bingos.filter(b => b.includes(index));
      possibleBingos.forEach(set => {
        let bingo = true;
        let setIdx = 0;
        while (bingo & (setIdx < set.length)) {
          bingo = bingo && this.squareStatus[set[setIdx++]];
        }
        if (bingo) {
          console.log(`Bingo on ${set}`);
        }
      });
    }
  },
  mounted() {
    this.$set(this.squareStatus, FREE_SPACE_INDEX, true);
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
