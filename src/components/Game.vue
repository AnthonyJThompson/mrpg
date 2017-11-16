<template>
  <div>
    <div>
      <!-- monster -->
      <div id="monster">
        <div id="monster-hide-table" v-if="monster.isAlive">
          <img :src="monster.img" />
          <small>{{ monster.name }}</small>
        </div>
      </div>
      <!-- health -->
      <div id="health">
        <small >{{ monster.health }} / {{ monster.maxHealth }}</small>
        <div id="monster-health">
          <div :style="{ width: (monster.health / monster.maxHealth) * 100 + '%'}"></div>
        </div>
      </div>
      <!-- controls -->
      <div id="controls">
        <button :disabled="!monster.isAlive" @click="attack">Attack</button>
        <button @click="restart">Re(Start)</button>
        <button :disabled="player.gold <= upgradeCost" @click="upgrade"><span>Buy stuff<br />({{ upgradeCost }} gold)</span></button>
      </div>
      <!-- stats -->
      <div id="stats">
        <input type="text" v-model="player.name" />
        <span>Name: {{ player.name }}</span>
        <span>Attack Power: {{ player.attack }}</span>
        <span>EXP: {{ player.exp }}</span>
        <span>Gold: {{ player.gold }}</span>
      </div>
    </div>
  </div>  
</template>

<script>
var playerStorage = {
  load: function () {
    var player
    try {
      player = JSON.parse(localStorage.getItem('player'))
    } catch (err) {
      player = null
    }
    if (!player) {
      player = {
        name: 'My dood',
        attack: 10,
        exp: 0,
        gold: 0
      }
    }
    return player
  },
  save: function (player) {
    localStorage.setItem('player', JSON.stringify(player))
  }
}

var monsterStorage = {
  load: function () {
    var monster
    try {
      monster = JSON.parse(localStorage.getItem('monster'))
    } catch (err) {
      monster = null
    }
    if (!monster) {
      monster = {
        name: 'bear',
        img: require('../assets/bear.png'),
        maxHealth: 100,
        health: 0,
        isAlive: false
      }
    }
    return monster
  },
  save: function (monster) {
    localStorage.setItem('monster', JSON.stringify(monster))
  }
}

export default {
  name: 'Game',
  data () {
    return {
      monster: monsterStorage.load(),
      player: playerStorage.load()
    }
  },
  computed: {
    upgradeCost: function () {
      return Math.round(this.player.attack * 2)
    },
    monsterGold: function () {
      return Math.round(this.monster.maxHealth / 30)
    },
    monsterExp: function () {
      return Math.round(this.monster.maxHealth / 50)
    }
  },
  methods: {
    attack: function () {
      var p = this.player
      var m = this.monster
      m.health -= p.attack
      if (m.health <= 0) {
        m.health = 0
        m.maxHealth += Math.round(m.maxHealth * 0.1)
        m.isAlive = false
        p.exp += this.monsterExp
        p.gold += this.monsterGold
      }
      playerStorage.save(p)
      monsterStorage.save(m)
    },
    restart: function () {
      this.monster.health = this.monster.maxHealth
      this.monster.isAlive = true
      playerStorage.save(this.player)
      monsterStorage.save(this.monster)
    },
    upgrade: function () {
      if (this.player.gold >= this.upgradeCost) {
        this.player.gold -= this.upgradeCost
        this.player.attack += 2
        playerStorage.save(this.player)
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
  max-width: 70%;
  height: 250px;
}
#monster-hide-table{
  height: 100%;
  display: inline-grid;
}
#monster img{
  margin: 0 auto;
  max-width: 90%;
  max-height: 235px;
  object-fit: contain;
}
#health{
  text-align: center;
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
  display: flex;
}
#controls button{
  height: 100px;
  width: 33.3%;
  max-width: 200px;
}
#stats{
  text-align: center;
  display: grid;
}
</style>
