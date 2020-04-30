<template>
  <div class="game-square" :style="styleObj" @click="toggleState">{{ text }}</div>
</template>

<script>
export default {
  props: {
    text: {
      type: String
    },
    checked: { type: Boolean },
    id: { type: Number }
  },
  methods: {
    toggleState() {
      this.$emit('toggle-state', this.id);
    }
  },
  computed: {
    styleObj() {
      const backgroundColor = this.checked ? 'blue' : 'white';
      const color = this.checked ? 'white' : 'black';
      return {
        backgroundColor,
        color
      };
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
  margin: 0 0.5rem;
  padding: 0.5rem;
  border-radius: 10px;
  height: 5rem;
  transition: all 200ms ease-in-out;
  overflow: hidden;
}
</style>
