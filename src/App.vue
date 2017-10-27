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
                <button id="special-attack" @click="specialAttack">SPECIAL ATTACK</button>
                <button id="heal" @click="heal">HEAL</button>
                <button id="give-up" @click="giveUp">GIVE UP</button>
            </div>
        </section>
        <section class="row log">
            <div class="small-12 columns">
                <ul>
                    <li>

                    </li>
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
export default {
  data () {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameOn: false
    }
  },
  methods: {
    startGame() {
      this.gameOn = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
    },
    attack() { 
      this.playerHealth -= this.damage(3, 30);
        if (this.monsterHealth <= 0) {
          alert("You won!");
          this.gameOn = false;
          return;
        }
      this.monsterHealth -= this.damage(3, 20);
        if (this.playerHealth <= 0) {
          alert("You lost!");
          this.gameOn = false;
        }
    },
    damage (min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
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
