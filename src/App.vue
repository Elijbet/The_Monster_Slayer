<template>
    <div id="app">
        <section class="row">
            <div class="small-6 columns">
                <h1 class="text-center">YOU</h1>
                <div class="healthbar">
                    <div 
                      class="healthbar text-center" 
                      style="background-color: green; margin: 0; color: white;" 
                      :style="{width: playerHealth + '%'}">
                      {{ playerHealth }}
                    </div>
                </div>
            </div>
            <div class="small-6 columns">
                <h1 class="text-center">MONSTER</h1>
                <div class="healthbar">
                    <div 
                      class="healthbar text-center" 
                      style="background-color: green; margin: 0; color: white;"
                      :style="{width: monsterHealth + '%'}">
                      {{ monsterHealth }}
                    </div>
                </div>
            </div>
        </section>
        <section class="row controls"  v-if="!gameOn">
            <div class="small-12 columns">
                <button id="start-game" @click="startGame">START NEW GAME</button>
            </div>
        </section>
        <section class="row controls" v-else>
            <div class="small-12 columns">
                <button id="attack" @click="attack">ATTACK</button>
                <button id="special-attack" @click="specialAttack" >SPECIAL ATTACK</button>
                <button id="heal" @click="heal">HEAL</button>
                <button id="give-up" @click="giveUp">GIVE UP</button>
            </div>
        </section>
        <section class="row log" v-if="turns.length > 0">
            <div class="small-12 columns">
                <ul>
                    <li v-for="item in turns" 
                        :class="{ 'player-turn': turns.isPlayer, 'monster-turn': !turns.isPlayer}">
                        {{ item.text }}
                    </li>
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
export default {
  data: function() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameOn: false,
      turns: []
    }
  },
  methods: {
    startGame: function() {
      this.gameOn = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.turns = []
    },
    attack: function() { 
      var damageLog = this.damage(3, 10);
      this.monsterHealth -= damageLog;
      this.turns.unshift({
        isPlayer: true,
        text: 'Player hits monster for ' + damageLog
      });
      if (this.checkWin()){
        return;
      }

      this.monsterAttacks();

    },
    specialAttack: function(){
      var damageLog = this.damage(3, 20);
      this.monsterHealth -= damageLog;
      this.turns.unshift({
        isPlayer: true,
        text: 'Player hits monster hard for ' + damageLog
      });
      if (this.checkWin()){
        return;
      }

      this.monsterAttacks();
    },
    heal: function(){
        
        if (this.playerHealth <= 90){
          this.playerHealth += 10;  
        } else { 
            this.playerHealth = 100;
        };

        this.turns.unshift({
            isPlayer: true,
            text: 'Player heals for ' + 10
          });
        this.monsterAttacks();
    },
    giveUp: function() {
      this.gameOn = false;
    },
    monsterAttacks: function(){
      var damageLog = this.damage(3, 12);
      this.playerHealth -= damageLog;
      this.turns.unshift({
        isPlayer: false,
        text: 'Monster hits player for ' + damageLog 
      });
      this.checkWin();
    },
    damage: function (min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
      },
    checkWin: function (){
        if (this.monsterHealth <= 0) {
          if (confirm('You won! New game?')){
            this.startGame();
          } else {
            this.gameOn = false;
          }
          return true;
        } else if (this.playerHealth <= 0) {
          if (confirm('You lost! New game?')){
            this.startGame();
          } else {
            this.gameOn = false;
          }
          return true;
        }
        return false;
    }
  }
}
</script>

<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
