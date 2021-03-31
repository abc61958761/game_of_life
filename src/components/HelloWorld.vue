<template>
  <div class="hello">
    <button @click="gameOfLife(board)">按鈕</button>
    {{board}}
    <div style="display: flex;flex-direction: column;">
      <div  style="display: flex" v-for="(item, index) in board" :key="index">
        <div class="basic" :class="[x === 1 ? 'back-color' : '']" v-for="(x, itemIndex) in item" :key="itemIndex"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: () => {
    return {
      board: [[0,1,0],[0,0,1],[1,1,1],[0,0,0]],
    }
  },
  mounted () {
    this.gameOfLife(this.board);
  },
  methods: {
    gameOfLife(board) {
      const result = new Array();
      for(let i=0; i < board.length; i++) {
        const newItem = new Array();
        for(let y=0; y < board[i].length; y++) {
            let n = 0;
          
            if (i-1 >= 0) {
                if (y-1 >= 0) {
                    n = this.getAliveNeighbors(board[i-1][y-1], n);
                }
                if (y+1 <  board[i].length) {
                    n = this.getAliveNeighbors(board[i-1][y+1], n);
                }
                n = this.getAliveNeighbors(board[i-1][y], n);
            }
            if (i+1 < board.length) {
                if (y-1 >= 0) {
                    n = this.getAliveNeighbors(board[i+1][y-1], n);
                }
                if (y+1 <  board[i].length) {
                    n = this.getAliveNeighbors(board[i+1][y+1], n);
                }
                n = this.getAliveNeighbors(board[i+1][y], n);
            }
            if (y-1 >= 0) {
                n = this.getAliveNeighbors(board[i][y-1], n);
            }
            if (y+1 <  board[i].length) {
                n = this.getAliveNeighbors(board[i][y+1], n);
            }
            if (board[i][y] === 1) {
                if (n == 2 || n == 3) {
                    newItem.push(1);
                } else {
                    newItem.push(0);
                }
            } else {
                if (n == 3) {
                    newItem.push(1);
                } else {
                    newItem.push(0);
                }
            }
        }
        result.push(newItem);
      }
      for(let i = 0; i<result.length; i++) {
          for(let y = 0; y< result[i].length; y++) {
              board[i][y] = result[i][y];
          }
      }
    },
    getAliveNeighbors(item, n) {
      return item == 1 ? n + 1 : n;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.basic {
  border: 1px solid black; 
  width: 15px; 
  height: 15px;
}
.back-color {
  background: black;
}
</style>
