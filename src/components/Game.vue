<template>
  <div v-if="loaded">
    <iframe src="https://www.extra-life.org/index.cfm?fuseaction=widgets.300x250thermo&participantID=295008" width="302" height="252" frameborder="0" scrolling="no"><a href="https://www.extra-life.org/index.cfm?fuseaction=donorDrive.participant&participantID=295008">Make a Donation!</a></iframe>
    <div>
      <div id="monster-list-container">
        <div v-for="m in monsterList" :key="m.id" :style="{ maxWidth: 100 / monsterList.length + '%' }">
          <!-- monster -->
          <div id="monster">
            <div id="monster-hide-table" v-if="m.isAlive">
              <img :disabled="!m.isAlive" @click="attack(m)" :src="m.img"/>
              <small>{{ m.name }}</small>
            </div>
          </div>
          <!-- health -->
          <div id="health">
            <div id="monster-health" :style="{ width: 160 / monsterList.length + 'px' }">
              <div :style="{ width: (m.health / m.maxHealth) * 100 + '%'}"></div>
            </div>
          </div>
        </div>
      </div>
      <!-- controls -->
      <div id="controls">
        <button :disabled="player.gold < upgradeCost" @click="upgrade"><span>Buy stuff ({{ upgradeCost }} gold)</span></button>
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
    var player = JSON.parse(localStorage.getItem('player'))
    return player
  },
  save: function (player) {
    localStorage.setItem('player', JSON.stringify(player))
  }
}

var monsterStorage = {
  loadList: function () {
    var monsterList = JSON.parse(localStorage.getItem('monsterList'))
    return monsterList
  },
  saveList: function (monsterList) {
    console.log('save list')
    localStorage.setItem('monsterList', JSON.stringify(monsterList))
  },
  load: function () {
    var monster = JSON.parse(localStorage.getItem('monster'))
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
      loaded: false,
      monsterList: monsterStorage.loadList(),
      monster: monsterStorage.load(),
      player: playerStorage.load()
    }
  },
  beforeCreate: function () {
    var version = '1'
    if (localStorage.getItem('version') != null && localStorage.getItem('version') === version) {
      return
    }

    var player = {
      name: 'My dood',
      attack: 10,
      exp: 0,
      gold: 0
    }
    localStorage.setItem('player', JSON.stringify(player))
    var monsterList = [{
      id: 1,
      name: 'bear',
      img: require('../assets/bear.png'),
      maxHealth: 100,
      health: 100,
      isAlive: true
    }, {
      id: 2,
      name: 'goblin',
      img: require('../assets/goblin.png'),
      maxHealth: 50,
      health: 50,
      isAlive: true
    }, {
      id: 3,
      name: 'super goblin',
      img: require('../assets/goblin.png'),
      maxHealth: 200,
      health: 200,
      isAlive: true
    }, {
      id: 4,
      name: 'super bear',
      img: require('../assets/bear.png'),
      maxHealth: 500,
      health: 500,
      isAlive: true
    }]
    localStorage.setItem('monsterList', JSON.stringify(monsterList))
    var monster = monsterList[Math.floor(Math.random(0, 2) * 2)]
    localStorage.setItem('monster', JSON.stringify(monster))

    localStorage.setItem('version', version)
  },
  mounted: function () {
    window.unload = this.save
    window.onblur = this.save
    window.onbeforeunload = this.save
    window.onfocus = this.load
    this.loaded = true
  },
  computed: {
    upgradeCost: function () {
      return Math.round(this.player.attack * 1.3)
    }
  },
  methods: {
    save: function () {
      monsterStorage.saveList(this.monsterList)
      monsterStorage.save(this.monster)
      playerStorage.save(this.player)
    },
    load: function () {
      this.monster = monsterStorage.load()
      this.player = playerStorage.load()
    },
    attack: function (m) {
      var p = this.player
      // var m = this.monster
      m.health -= p.attack
      if (m.health <= 0) {
        m.health = 0
        m.maxHealth += Math.round(m.maxHealth * 0.1)
        m.isAlive = false
        p.exp += Math.round(m.maxHealth / 50)
        p.gold += Math.round(m.maxHealth / 30)

        // this.monster = this.monsterList[Math.floor(Math.random(0, 4) * 4)]
        console.log(m.name)
        m.health = m.maxHealth
        m.isAlive = true
        playerStorage.save(p)
        monsterStorage.save(m)
      }
      console.log(this.upgradeCost)
    },
    upgrade: function () {
      if (this.player.gold >= this.upgradeCost) {
        this.player.gold -= this.upgradeCost
        this.player.attack += Math.round(this.player.attack * 0.2)
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
  min-height: 110px;
}
#monster-list-container{
  display:-webkit-box;
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
#monster img:hover{
  filter: drop-shadow(0 0 8px black)
}
#monster img:active{
  max-width: 88%;
}
#health{
  text-align: center;
}
#monster-health{
  border: 3px solid;
  margin: 0 auto 10px auto;
}
#monster-health div{
  height: 10px;
  background-color: red;
}
#controls{
  text-align: center;
}
#controls button{
  height: 100px;
  width: 33.3%;
  max-width: 200px;
  vertical-align: middle;
}
#stats{
  text-align: center;
  display: grid;
}
</style>
