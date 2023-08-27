<template >

  <SliderM_Y />

</template>

<script>
import SliderM_Y from './components/SliderM_Y'

export default {
  name: 'App',
  components: {
    SliderM_Y,

  },
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #e8e8e8;
  margin-top: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100%;
}

</style>
