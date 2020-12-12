<template>
  <div id="app">
    <word-cloud></word-cloud>
  </div>
</template>

<script>
import wordCloud from './components/wordCloud'
document.body.parentNode.style.overflowY = "hidden";
document.body.parentNode.style.overflowX = "hidden";
export default {
  name: 'App',
  data(){
    return{
      
    }
  },
  components: {
    wordCloud
  }
}
</script>

<style lang='scss'>
@font-face {
  font-family: 'handfont';
  src: url('static/喜鹊在山林.TTF');
  font-weight: normal;
  font-style: normal;
}

body{
  margin:0px;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
