<template>
  <v-container>
    <v-layout text-center wrap>
      <v-flex xs12>
        <v-img
          :src="require('../assets/monster-logo.jpg')"
          class="my-3"
          contain
          height="180"
        ></v-img>
      </v-flex>
 
      <v-flex mb-4>
          
        <h1 class="display-1 font-weight-bold mb-3">
          Welcome to Monster Slayer Game
        </h1>
        <!-- <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a href="https://community.vuetifyjs.com" target="_blank">Discord Community</a>
        </p> -->
      </v-flex>

      <v-flex xs12>

    <!-- <div class="d-inline mx-2 red white--text">d-inline</div>
    <div class="d-inline pa-2 black white--text">d-inline</div> -->


        <v-card class="d-flex align-content-center flex-wrap">
      <div class="small-6">
              <h1 class="text-center">Your Health</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: red; margin: 0; color: black; border-radius: 10px;"
                        :style="{width: playerHealth + '%'}">
                    {{ playerHealth }}
                </div>
            </div>
        </div>
        <div class="small-6">
            <h1 class="text-center">Monster's Health</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: red; margin: 0; color: black; border-radius: 10px;"
                        :style="{width: monsterHealth + '%'}">
                    {{ monsterHealth }}
                </div>
            </div>
        </div>
        </v-card>


    <section class="row mx-12">
        <div class="small-6">
                <h1 class="text-center">Energy</h1>
              <div class="healthbar">
                  <div
                          class="healthbar text-center"
                          style="background-color: blue; margin: 0; color: black; border-radius: 10px;"
                          :style="{width: playerEnergy + '%'}">
                      {{ playerEnergy }}
                  </div>
              </div>
          </div>
          <div class="small-6">
              <h1 class="text-center">Energy</h1>
              <div class="healthbar">
                  <div
                          class="healthbar text-center"
                          style="background-color: blue; margin: 0; color: black; border-radius: 10px;"
                          :style="{width: monsterEnergy + '%'}">
                      {{ monsterEnergy }}
                  </div>
              </div>
          </div>
      </section>
    <section class="controls" v-if="!gameIsRunning">
        <div class="small-12">
            <button id="start-game" @click="startGame">START NEW GAME</button>
        </div>
    </section>
    <section class="controls" v-else>
        <div class="small-12">
            <button id="attack" @click="attack">ATTACK (-10 energy)</button>
            <button id="special-attack" @click="specialAttack">SPECIAL ATTACK (-25 energy)</button>
            <button id="heal" @click="heal">HEAL (-5 energy)</button>
            <button id="addEnergy" @click="reChargeEnergy">Add 10 Energy</button>
            <button id="give-up" @click="giveUp">GIVE UP</button>
        </div>
    </section>
    <section class="log" v-if="turns.length > 0">
        <div class="small-12">
            <ul>
                <li v-for="turn in turns" V-bind:key = "turn.id"
                    :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}">
                    {{ turn.text }}
                </li>
            </ul>
        </div>
    </section>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import {
    VContainer, 
    VLayout, 
    // VFlex, 
    VImg
    } from 'vuetify/lib'
export default {
    name: 'HelloWorld',
    components: {
        VContainer,
        VLayout,
        // VFlex,
        VImg
    },
    data: () => ({
        playerHealth: 100,
        monsterHealth: 100,
        playerEnergy: 100,
        monsterEnergy: 100,
        gameIsRunning: false,
        turns: []
    }),
methods: {
        startGame: function () {
            this.gameIsRunning = true;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            this.playerEnergy = 100;
            this.monsterEnergy = 100;
            this.turns = [];
        },
        enchanced: function() {
            return 5;
        },
        attack: function () {
            var damage = this.calculateDamage(3, 10);
            var energy = this.calculateDamage(10, 10);
            damage += this.enchanced();
            this.monsterHealth -= damage;
            this.playerEnergy -= energy;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster for ' + damage
            });
            if (this.checkWin()) {
                return;
            }
            setTimeout(() => {
                this.monsterAttacks();
            }, 1000);
        },
        specialAttack: function () {
            var damage = this.calculateDamage(10, 20);
            var energy = this.calculateDamage(25, 25);
            this.playerEnergy -= energy;
            this.monsterHealth -= damage;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster hard for ' + damage
            });
            if (this.checkWin()) {
                return;
            }
            setTimeout(() => {
                this.monsterAttacks();
            }, 1000);
        },
        heal: function () {
            var energy = this.calculateDamage(5, 5);
            this.playerEnergy -= energy;
            if (this.playerHealth <= 90) {
                this.playerHealth += 10;
            } else {
                this.playerHealth = 100;
            }
            this.turns.unshift({
                isPlayer: true,
                text: 'Player heals for 10'
            });
            setTimeout(() => {
                this.monsterAttacks();
            }, 1000);
        },
        reChargeEnergy: function() {
            var energy = 10;
            this.playerEnergy += energy;
            if (this.playerEnergy <= 100) {
                this.playerEnergy += 0;
            } else {
                this.playerEnergy = 100;
            }
            if (this.checkWin()) {
                return;
            }
            setTimeout(() => {
                this.monsterAttacks();
            }, 1000);
        },
        giveUp: function () {
            (confirm('Oh why did you give up!?'))
            this.gameIsRunning = false;
        },
        monsterAttacks: function() {
            var damage = this.calculateDamage(5, 9);
            var energy = this.calculateDamage(5, 5);
            this.monsterEnergy -= energy;
            this.playerHealth -= damage;
            this.checkWin();
            this.turns.unshift({
                isPlayer: false,
                text: 'Monster hits Player for ' + damage
            });
        },
        calculateDamage: function(min, max) {
            return Math.max(Math.floor(Math.random() * max) + 1, min);
        },
        calculateEnergy: function(min, max) {
            return Math.max(Math.floor(Math.random() * max) + 1, min);
        },
        checkWin: function() {
            if (this.monsterHealth <= 0) {
                if (confirm('You won! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            } else if (this.playerHealth <= 0) {
                if (confirm('You lost! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            } else if (this.monsterEnergy <= 0) {
                if (confirm('You exhausted the monster and won! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            } else if (this.playerEnergy <= 0) {
                if (confirm('You ran out of energy! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            }
            return false;
        }
    }
};
</script>






<style>

.border {
    border-radius: 15;
}

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


