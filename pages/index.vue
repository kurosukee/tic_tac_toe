<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <div class="grey--text mb-2 display-1">
      マルバツゲーム
    </div>
    <!-- Grid -->
    <v-container class="grey  ma-5">
      <v-row no-gutters>
        <template v-for="n in level * level">
          <v-col :key="n">
            <v-card
              outlined
              tile
              @click="clickGrid(n)"
            >
              <v-card-title
                class="justify-center display-1"
                :class="pickGridColor(n)"
              >
                {{ pickGridText(n) }}
              </v-card-title>
            </v-card>
          </v-col>
          <v-responsive
            v-if="(n % level) === 0"
            :key="`width-${n}`"
            width="100%"
          ></v-responsive>
        </template>
      </v-row>
    </v-container>
    <!-- Button -->
    <v-flex xs12 sm8 md6>
      <v-row justify="space-around" class="ma-2">
        <b class="headline" v-if="this.winner === undefined">{{ players[turn].name }}'s turn!</b>
        <b class="headline orange--text" v-if="this.winner !== undefined">WINNER!! {{ players[turn].name }}</b>
      </v-row>
      <v-row justify="space-around">
        <div>
          <v-btn
            class="mx-1"
            :color="players[0].color"
            :disabled="!(turn === 0)"
          >
            Player1: {{ players[0].name }}
          </v-btn>
          <v-btn
            class="mx-1"
            :color="players[1].color"
            :disabled="!(turn === 1)"
          >
            Player2: {{ players[1].name }}
          </v-btn>
        </div>
      </v-row>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  components: {},
  data () {
    return {
      messages: 0,
      show: false,
      turn: 0,
      players: [{name:'Tom', color:'blue', disabled:false}, {name:'Jerry', color:'red', disabled:true}],
      level: 3,
      grid: [],
      pick: {0:[], 1:[]},
      player: '',
      winner: undefined,
    }
  },
  methods: {
    clickGrid: function (row) {
      if (this.grid[row] !== undefined) return;
      this.grid[row] = this.turn;
      this.pick[this.turn].push(row);
      this.judgment(row);
      this.switchPlayer();
    },
    pickGridColor: function (row) {
      if (this.grid[row]=== undefined) return;
      return this.players[this.grid[row]].color+'--text';
    },
    pickGridText: function (row) {
      if (this.grid[row] === undefined) return "　";
      if(this.grid[row]===0) return "○";
      if(this.grid[row]===1) return "×";
    },
    switchPlayer: function () {
      switch(true) {
        case (this.winner !== undefined) : break;
        case (this.turn === 0) : this.turn = 1;break;
        case (this.turn === 1) : this.turn = 0;break;
      }
    },
    judgment: function (row) {
      if (this.winner !== undefined) return;
      var pattern = [
        [1,2,3],
        [4,5,6],
        [7,8,9],
        [1,4,7],
        [2,5,8],
        [3,6,9],
        [1,5,9],
        [3,5,7],
      ];
      for(var key in pattern) {
        var pickCurrentTurn = this.pick[this.turn];
        var result = pickCurrentTurn.filter((value) => (pattern[key].indexOf(value) !== -1));
        if (result.length === 3) {
          this.winner = this.turn;
          break;
        };
      }
    },
  },
}
</script>
