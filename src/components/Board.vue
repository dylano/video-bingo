<template>
  <div class="game-board">
    <span class="label-letter">V</span>
    <span class="label-letter">I</span>
    <span class="label-letter">D</span>
    <span class="label-letter">E</span>
    <span class="label-letter">O</span>
    <game-square
      v-for="(item, idx) in randomPhrases"
      :key="idx"
      :id="idx"
      :text="item"
      :checked="squareStatus[idx]"
      @toggle-state="toggleState"
    />
    <span class="label-letter">B</span>
    <span class="label-letter">I</span>
    <span class="label-letter">N</span>
    <span class="label-letter">G</span>
    <span class="label-letter">O</span>
  </div>
</template>

<script>
import GameSquare from './GameSquare';
const FREE_SPACE_INDEX = 12;
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
      copy[12] = `FREE SPACE`;
      return copy;
    }
  },
  methods: {
    toggleState(idx) {
      this.squareStatus = this.squareStatus.map((val, index) => {
        if (index === FREE_SPACE_INDEX) {
          return true;
        } else if (index === idx) {
          return !val;
        } else {
          return val;
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
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  background-color: grey;
  height: 100vh;
}

.label-letter {
  font-size: 3rem;
  font-weight: bold;
  color: white;
  text-align: center;
}
</style>
