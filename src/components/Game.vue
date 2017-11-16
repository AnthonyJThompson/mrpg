<template>
  <div>
    <!-- monster -->
    <div id="monster">
      <div><img :src="monster.img" /></div>
      <small>{{ monster.name }}</small>
    </div>
    <!-- health -->
    <div id="health">
      <div id="monster-health">
        <div></div>
      </div>
    </div>
    <!-- controls -->
    <div id="controls"></div>
  </div>  
</template>

<script>
export default {
  name: 'Game',
  data () {
    return {
      msg: 'something',
      monster: {
        name: 'bear',
        img: require('../assets/bear.png'),
        health: 10
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#monster{
  margin: 0 auto;
  text-align: center;
}
#monster-health{
  border: 5px solid;
  width: 200px;
  margin: 0 auto 10px auto;
}
</style>
