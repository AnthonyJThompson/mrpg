<template>
  <div>
    <!-- monster -->
    <div id="monster">
      <div v-if="monster.isAlive">
        <div><img :src="monster.img" /></div>
        <div><small>{{ monster.name }}</small></div>
      </div>
    </div>
    <!-- health -->
    <div id="health">
      <div id="monster-health">
        <div :style="{ width: (monster.health) + '%'}"></div>
      </div>
    </div>
    <!-- controls -->
    <div id="controls">
      <button v-if="monster.isAlive" @click="attack">Attack</button>
      <button @click="restart">Re(start)</button>
    </div>
    <!-- stats -->
    <div id="stats">
      <p>Name: {{ player.name }}</p>
      <p>Attack Power: {{ player.attack }}</p>
      <p>EXP: {{ player.exp }}</p>
      <p>Gold: {{ player.gold }}</p>
    </div>
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
        maxHealth: 100,
        health: 0,
        isAlive: false,
        exp: 1,
        gold: 2
      },
      player: {
        name: 'My dood',
        attack: 10,
        exp: 0,
        gold: 0
      }
    }
  },
  methods: {
    attack: function () {
      var p = this.player
      var m = this.monster
      m.health -= p.attack
      if (m.health <= 0) {
        m.health = 0
        m.isAlive = false
        p.exp += m.exp
        p.gold += m.gold
      }
    },
    restart: function () {
      this.monster.health = this.monster.maxHealth
      this.monster.isAlive = true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#monster{
  margin: 0 auto;
  text-align: center;
  height: 450px;
}
#monster-health{
  border: 5px solid;
  width: 200px;
  margin: 0 auto 10px auto;
}
#monster-health div{
  height: 20px;
  background-color: red;
}
#controls{
  text-align: center;
}
#controls *{
  height: 100px;
  width: 30%;
  max-width: 200px;
}
#stats{
  text-align: center;
}
</style>
