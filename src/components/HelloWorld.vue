<template>
  <v-container class="background">
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
      </v-flex>

      <v-flex xs12>
        <v-card class="spacing row mx-12">
      <div class="small-6">
              <h1 class="text-center">Your Health</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: red; margin: 0; color: black; border-radius: 5px;"
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
                        style="background-color: red; margin: 0; color: black; border-radius: 5px;"
                        :style="{width: monsterHealth + '%'}">
                    {{ monsterHealth }}
                </div>
            </div>
        </div>
        </v-card>


    <v-card class="spacing row mx-12">
        <div class="small-6">
                <h1 class="text-center">Your Energy</h1>
              <div class="healthbar">
                  <div
                          class="healthbar text-center"
                          style="background-color: blue; margin: 0; color: black; border-radius: 5px;"
                          :style="{width: playerEnergy + '%'}">
                      {{ playerEnergy }}
                  </div>
              </div>
          </div>
          <div class="small-6">
              <h1 class="text-center">Monster's Energy</h1>
              <div class="healthbar">
                  <div
                          class="healthbar text-center"
                          style="background-color: blue; margin: 0; color: black; border-radius: 5px;"
                          :style="{width: monsterEnergy + '%'}">
                      {{ monsterEnergy }}
                  </div>
              </div>
          </div>
      </v-card>
    <v-card class="controls" v-if="!gameIsRunning">
        <div class="small-12">
            <button id="start-game" @click="startGame">START NEW GAME</button>
        </div>
    </v-card>
    <v-card class="controls" v-else>
        <div class="small-12">
            <!-- <button id="attack" @click="attack">ATTACK (-10 energy)</button> -->
            <!-- <button id="special-attack" @click="specialAttack">SPECIAL ATTACK (-25 energy)</button> -->
            <!-- <button id="heal" @click="heal">HEAL (-5 energy)</button> -->
            <!-- <button id="addEnergy" @click="reChargeEnergy">Add 10 Energy</button> -->
            <!-- <button id="give-up" @click="giveUp">GIVE UP</button> -->

            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-btn dark v-on="on" id="attack" @click="attack">Attack</v-btn>
                </template>
                    <span class="tooltipColor">Energy -10</span>
            </v-tooltip>

            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-btn dark v-on="on" id="special-attack" @click="specialAttack">SPECIAL ATTACK</v-btn>
                </template>
                    <span class="tooltipColor">Energy -25</span>
            </v-tooltip>

            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-btn dark v-on="on" id="heal" @click="heal">HEAL</v-btn>
                </template>
                    <span class="tooltipColor">Energy -5</span>
            </v-tooltip>

            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-btn dark v-on="on" id="addEnergy" @click="reChargeEnergy">Energy</v-btn>
                </template>
                    <span class="tooltipColor">Give yourself a break to increase +10 energy</span>
            </v-tooltip>

            <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                    <v-btn dark v-on="on" id="give-up" @click="giveUp">GIVE UP</v-btn>
                </template>
                    <span class="tooltipColor">Don't dare to think about it..</span>
            </v-tooltip>

        </div>
    </v-card>
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
    <v-footer
    dark
    padless
  >
    <v-card
      class="flex"
      flat
      tile
    >
      <v-card-title class="charcoal">
        <strong class="subheading">Get connected with us on social networks!</strong>

        <div class="flex-grow-1"></div>

        <v-btn
          v-for="icon in icons"
          :key="icon"
          class="mx-4"
          dark
          icon
        >
          <v-icon size="24px">{{ icon }}</v-icon>
        </v-btn>
      </v-card-title>

      <v-card-actions class="grey justify-center">
        {{ new Date().getFullYear() }} — <strong>MONSTER Inc.</strong>
      </v-card-actions>
    </v-card>
  </v-footer>
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
        turns: [],
        icons: [
        'fab fa-facebook',
        'fab fa-twitter',
        'fab fa-google-plus',
        'fab fa-linkedin',
        'fab fa-instagram',
      ],
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

.tooltipColor {
    color: white;
    font-size: 14px;
    border-radius: 5px;
    padding: 5px;
}

.background {
    background-color: grey;
}

.spacing {
    padding-bottom: 15px;
}

.text-center {
    text-align: center;
    margin: 0px;
}

.healthbar {
    width: 90%;
    height: 35px;
    background-color: rgb(41, 40, 40);
    margin: auto;
    transition: width 500ms;
    border-width: 1px;
}

.controls, .log {
    margin-top: 15px;
    text-align: center;
    padding: 0px;
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
    transition: 0.5s;
    font-size: 20px;
    background-color: rgb(44, 39, 39);
    padding: 6px;
    box-shadow: 0 2px 2px black;
    margin: 10px;
    color: black;
}

#start-game {
    background-color: rgb(245, 113, 113);
    border-radius: 10px;
}

#start-game:hover {
    background-color: red;
}

#attack {
    background-color: #ff7367;
    border-radius: 10px;
    color: black;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: rgb(246, 201, 117);
    border-radius: 10px;
    color: black;
}

#special-attack:hover {
    background-color: orange;
}

#heal {
    background-color: #aaffb0;
    border-radius: 10px;
    color: black;
}

#heal:hover {
    background-color: #76ff7e;
}

#addEnergy {
    background-color: rgb(128, 128, 248);
    border-radius: 10px;
    color: black;
}

#addEnergy:hover {
    background-color: blue;
    border-radius: 10px;
}

#give-up {
    background-color: #ffffff;
    border-radius: 10px;
    color: black;
}

#give-up:hover {
    background-color: black;
    color: white;
}

</style>


