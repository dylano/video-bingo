<template>
  <div id="app">
    <Board :phrases="phrases" />
  </div>
</template>

<script>
import { phrases } from './lib/phrases';
import Board from './components/Board';
export default {
  name: 'App',
  components: { Board },
  data() {
    return {
      phrases
    };
  }
};
</script>

<style>
:root {
  --zoom-color: #0c63ce;
}

* {
  box-sizing: border-box;
}

body,
html {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

body {
  background-color: darkgrey;
}
</style>
