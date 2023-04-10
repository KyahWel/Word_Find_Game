<template>
  <div class="main-container-game">
    <div class="header">
      <a v-b-modal.modal-multi-game-settings><i class="fa-solid fa-gears"></i></a>
    </div>
    <div class="main-content">
      <div class="grid">
        <div class="left">
          <div class="board">
            <b-row v-for="row in numRows" :key="row">
              <b-col v-for="cols in numCols" :key="cols" :data-row="row" :data-column="cols" class="col-design cell-data">
              </b-col>
            </b-row>
          </div>
        </div>
        <div class="right">
          <h1>{{minutes}}:{{ seconds }}</h1>
          <b-container>
            <b-row class="text-center">
              <b-col v-for="word in words" :key="word" cols="4">
                {{word}}
              </b-col>
            </b-row>
          </b-container>
        </div>
      </div>
      <b-modal id="modal-multi-game-settings" no-close-on-backdrop centered hide-footer hide-header>
        <template #default="{ close }">
              <b-button @click="close()" block class="button-play">
                <h3>CONTINUE</h3>
              </b-button>
            
            <b-button v-b-modal.modal-multi-2 block class="button-quit-game">
              <h3>QUIT GAME</h3>
            </b-button>
</template>
        </b-modal>

        <b-modal id="modal-multi-2" no-close-on-backdrop centered hide-footer>
          <h2>Are you sure?</h2>
          <b-button v-b-modal.modal-multi-1 size="sm" block class="button-quit-game">
            <h3>QUIT GAME</h3>
          </b-button>
        </b-modal>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['numCols', 'numRows', 'words'],
    data() {
      return {
        timerCount: 120, //In Seconds
        minutes: 2,
        seconds: "00",
        remainingWords: 10,
        tempWords: []
      }
    },
    watch: {
      timerCount: {
        handler(value) {
          if (value > 0) {
            setTimeout(() => {
              this.timerCount--;
              this.minutes = Math.floor(this.timerCount / 60);
              this.seconds = String(this.timerCount > 59 ? this.timerCount - 60 : this.timerCount)
              this.seconds = this.seconds.length === 1 ? "0" + this.seconds : this.seconds
            }, 1000);
          }
        },
        immediate: true
      }
    },
    methods: {
      checkCellOccupied(word, startPosition, orientation) {
        var status = ''
        var incrementBy = 0
        const individuals = document.querySelectorAll('.cell-data');
        if (orientation === 'row') {
          incrementBy = 1
        } else if (orientation === 'column') {
          incrementBy = this.numCols
        }
        for (let p = startPosition, q = 0; q < word.length; q++) {
          if (individuals[p].hasAttribute('data-word')) {
            status = 'occupied'
            break
          } else {
            status = 'empty'
          }
          p += incrementBy
        }
        return status
      },
      placeCorrectWords(wordsArray) {
        var positions = ['row', 'column']
        var nextLetter = 0
        var newStartPoint = 0
        for (let i = 0; i < wordsArray.length; i++) {
          var getOrientation = positions[Math.floor(Math.random() * positions.length)]
          const individuals = document.querySelectorAll('.cell-data');
          const start = Math.floor(Math.random() * individuals.length)
          const startRow = individuals[start].getAttribute('data-row');
          const startColumn = individuals[start].getAttribute('data-column');
          if (getOrientation === 'row') {
            nextLetter = 1
            if ((startColumn * 1) + wordsArray[i].length <= this.numCols) {
              newStartPoint = start
            } else {
              var newColumnPoint = this.numCols - wordsArray[i].length - 1
              newStartPoint = ((startRow - 1) * this.numCols) + (newColumnPoint - 1)
            }
          } else if (getOrientation === 'column') {
            nextLetter = this.numCols
            if ((startRow * 1) + wordsArray[i].length <= this.numRows) {
              newStartPoint = start
            } else {
              var newRowPoint = this.numRows - wordsArray[i].length - 1
              newStartPoint = newRowPoint * startColumn
            }
          }
          var characters = wordsArray[i].split("");
          var nextPosition = 0
          var isCellOccupied = this.checkCellOccupied(wordsArray[i], newStartPoint, getOrientation)
          if (isCellOccupied === 'empty') {
            characters.forEach(item => {
              individuals[newStartPoint + nextPosition].innerText = item
              individuals[newStartPoint + nextPosition].setAttribute('data-word', wordsArray[i])
              individuals[newStartPoint + nextPosition].style.color = "red";
              nextPosition += nextLetter;
            });
          } else {
            this.tempWords.push(wordsArray[i])
          }
        }
      },
      generateRandomLetter() {
        const individuals = document.querySelectorAll('.cell-data');
        var alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
        for (let i = 0; i < individuals.length; i++) {
          if (!individuals[i].hasAttribute('data-word')) {
            individuals[i].innerText = alphabets.charAt(Math.floor(Math.random() * 26))
          }
        }
      },
    },
    mounted() {
      this.placeCorrectWords(this.words)
      this.placeCorrectWords(this.tempWords)
      this.generateRandomLetter()
    },
  }
</script>



<style scoped>
  .main-container-game {
    background-color: white;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    width: 100%;
    padding: 0;
    margin: 0;
  }
  .header {
    display: flex;
    /* border: 1px solid red; */
    width: 100%;
    justify-content: flex-end;
    padding: 10px 20px;
  }
  .main-content {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    flex-grow: 1;
  }
  .grid {
    display: flex;
    /* border: 1px solid red; */
    width: 100%;
  }
  .left {
    display: flex;
    /* border: 1px solid red; */
    width: 100%;
    display: flex;
    justify-content: center;
  }
  .right {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    /* border: 1px solid red; */
    width: 100%;
  }
  .col-design {
    border: 1px solid black;
    width: 40px;
    height: 40px;
  }
  .menu .button-design-settings {
    font-family: 'Coiny', cursive;
    font-weight: 800;
    border: 2px solid white;
    font-size: 2em;
    padding: 8px 2px;
    width: 30%;
    text-shadow: 2px 2px 4px #000000;
    box-shadow: 2px 2px 4px #000000;
  }
  a i {
    font-size: 2em;
  }
  #modal-multi-2 h2 {
    text-align: center;
  }
  /* BUTTONS CSS */
  button {
    height: 60px;
    border-radius: 15px;
    border: none;
    color: #fff;
    transition: 0.3s ease;
    font-weight: 800;
    font-family: 'Coiny', cursive;
    font-size: 2em;
    -webkit-text-stroke: .5px #000000;
  }
  .button-play {
    background: linear-gradient(#3ffa6e 25%, #0ae641 50%);
    border-radius: 10px;
    margin-bottom: 25px;
    box-shadow: 0 10px #3ffa6e;
  }
  .button-play:hover {
    box-shadow: 0 8px #3ffa6e;
    transform: translateY(1px);
  }
  .button-play:active {
    box-shadow: 0 5px #3ffa6e;
    transform: translateY(4px);
  }
  .button-settings {
    background: linear-gradient(#6d9fb6 25%, #397691 50%);
    border-radius: 10px;
    margin-bottom: 25px;
    box-shadow: 0 10px #6d9fb6;
  }
  .button-settings:hover {
    box-shadow: 0 8px #6d9fb6;
    transform: translateY(1px);
  }
  .button-settings:active {
    box-shadow: 0 5px #6d9fb6;
    transform: translateY(4px);
  }
  .button-show-credits {
    background: linear-gradient(#6d9fb6 25%, #397691 50%);
    border-radius: 10px;
    margin-bottom: 25px;
    box-shadow: 0 10px #6d9fb6;
  }
  .button-show-credits:hover {
    box-shadow: 0 8px #6d9fb6;
    transform: translateY(1px);
  }
  .button-show-credits:active {
    box-shadow: 0 5px #6d9fb6;
    transform: translateY(4px);
  }
  .button-quit-game {
    background: linear-gradient(#f75362 25%, #dc3545 50%);
    border-radius: 10px;
    box-shadow: 0 10px #f6727e;
  }
  .button-quit-game:hover {
    box-shadow: 0 8px #f6727e;
    transform: translateY(1px);
  }
  .button-quit-game:active {
    box-shadow: 0 5px #f6727e;
    transform: translateY(4px);
  }
</style>