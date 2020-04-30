<template>
  <div class="game-square" :style="styleObj" @click="squareClicked" v-html="text" />
</template>

<script>
const statusStyle = {
  0: { color: 'black', backgroundColor: 'white', border: '3px solid white' },
  1: { color: 'white', backgroundColor: 'var(--zoom-color)', border: '3px solid white' },
  2: { color: 'yellow', backgroundColor: 'var(--zoom-color)', border: '3px solid yellow' }
};

export default {
  props: {
    text: {
      type: String
    },
    status: { type: Number },
    id: { type: Number }
  },
  methods: {
    squareClicked() {
      this.$emit('toggle-state', this.id);
    }
  },
  computed: {
    styleObj() {
      return statusStyle[this.status];
    }
  }
};
</script>

<style scoped>
.game-square {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  border-radius: 1rem;
  height: 6rem;
  transition: all 200ms ease-in-out;
  overflow: hidden;
}
</style>
