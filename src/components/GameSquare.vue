<template>
  <div class="game-square" :style="styleObj" @click="toggleState">
    {{ text }}
  </div>
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
      const backgroundColor = this.checked ? 'var(--zoom-color)' : 'white';
      const color = this.checked ? 'white' : 'black';
      const border = this.checked ? '3px solid yellow' : '3px solid white';
      return {
        backgroundColor,
        color,
        border
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
  border-radius: 1rem;
  height: 6rem;
  transition: all 200ms ease-in-out;
  overflow: hidden;
}
</style>
