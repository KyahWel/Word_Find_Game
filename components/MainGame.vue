<template>
  <div class="main-container-game">
    <div class="header">
      <a v-b-modal.modal-multi-game-settings><i class="fa-solid fa-gears"></i></a>
      <a class="btn" href="#open-modal-success">👋 GREAT JOB MODAL</a>
      <!-- <a class="btn" href="#open-modal-failed">👋 OH NO TANGA KA BUTTON</a> -->
    </div>
    <div v-if="failed" id="open-modal-success" class="modal-window-success">
      <div>
        <a href="#" title="Close" class="modal-close-success">Close</a>
        <h1>GREAT JOB!!</h1>
        <h3>YOU'VE FINISHED THE PUZZLE</h3>
        <div class="modal-buttons">
          <b-button class="button-play">PLAY AGAIN</b-button>
          <b-button class="button-quit-game">EXIT GAME</b-button>
        </div>
      </div>
    </div>
    <div v-if="failed" id="open-modal-failed" class="modal-window-failed">
      <div>
        <a href="#"  title="Close" class="modal-close-failed">Close</a>
        <h1>OH NO!</h1>
        <h3>YOU'VE RUN OUT OF TIME! AMBOBO MO KASI</h3>
        <div class="modal-buttons">
          <b-button class="button-play">TRY AGAIN</b-button>
          <b-button class="button-quit-game">EXIT GAME</b-button>
        </div>
      </div>
    </div>
    <div class="main-content">
      <div class="grid">
        <div class="left">
          <div class="board" id="board">
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

  props:['words','time'],
  data(){
    return{
      numRows: this.words.length * 2,
      numCols: this.words.length * 2 ,
      timerCount: this.time, 
      minutes: Math.floor(this.time / 60),
      seconds: this.time>=60 ? "00" : String(this.time),
      tempWords: [],
      remainingWords: this.words.length
      }
    },
    computed: {
    failed: function() {
      console.log(this.timerCount === 0)
      return this.timerCount === 0;
      },
    success: function() {
      console.log(this.remainingWords === 0)
      return this.remainingWords === 0;
      }
    },
    watch: {
      timerCount: {
        handler(value) {
          if (value > 0) {
            setTimeout(() => {
              this.timerCount--;
              this.minutes = Math.floor(this.timerCount / 60);
              this.seconds = String(this.timerCount > 59 ? this.timerCount - (60*(this.minutes)) : this.timerCount)
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
        } else if (orientation === 'diagonal') {
          incrementBy = this.numCols + 1
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
        var positions = ['row', 'column', 'diagonal']
        var nextLetter = 0
        var newStartPoint = 0
        const cells = document.querySelectorAll('#board .cell-data');
        var arr = Array.prototype.slice.call(cells);
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
              var newColumnPoint = this.numCols - wordsArray[i].length
              //New row Starting Point
              newStartPoint = arr.indexOf(document.querySelector(`.cell-data[data-row="${startRow}"][data-column="${newColumnPoint}"]`))
            }
          } 
          else if(getOrientation === 'column'){
            nextLetter = this.numCols
            if((startRow*1)+ wordsArray[i].length <= this.numRows){
              newStartPoint = start
            }
            else{
              var newRowPoint = this.numRows - wordsArray[i].length
              newStartPoint = arr.indexOf(document.querySelector(`.cell-data[data-row="${newRowPoint}"][data-column="${startColumn}"]`))
            }
          }
          else if(getOrientation === 'diagonal'){
          nextLetter = 1 + this.numCols
          if((startColumn*1)+ wordsArray[i].length <= this.numCols && 
          (startRow*1)+ wordsArray[i].length <= this.numRows){
            newStartPoint = start
          }
          //Column
          if((startColumn*1)+ wordsArray[i].length > this.numCols){
            var newColStartPoint = this.numCols - wordsArray[i].length
            newStartPoint = arr.indexOf(document.querySelector(`.cell-data[data-row="${startRow}"][data-column="${newColStartPoint}"]`))
          }
           //Row
           if((startRow*1)+ wordsArray[i].length > this.numRows ){
            var newRowStartPoint= (this.numRows - wordsArray[i].length)+1
            newStartPoint = arr.indexOf(document.querySelector(`.cell-data[data-row="${newRowStartPoint}"][data-column="${startColumn}"]`))
          }
          if((startColumn*1)+ wordsArray[i].length > this.numCols && 
          (startRow*1)+ wordsArray[i].length > this.numRows){
            var newColStartPoint = this.numRows - wordsArray[i].length
            var newRowStartPoint= this.numCols - wordsArray[i].length
            newStartPoint = arr.indexOf(document.querySelector(`.cell-data[data-row="${newRowStartPoint}"][data-column="${newColStartPoint}"]`))
          }
        }
          var characters = wordsArray[i].split("");
          var nextPosition = 0
          var isCellOccupied = this.checkCellOccupied(wordsArray[i], newStartPoint, getOrientation)
          if(isCellOccupied === 'empty'){
              characters.forEach(item => {
              individuals[newStartPoint + nextPosition].innerText = item
              individuals[newStartPoint + nextPosition].setAttribute('data-word', wordsArray[i])
              //individuals[newStartPoint + nextPosition].style.backgroundColor = "rgba(88, 248, 73, 0.507)";
              nextPosition += nextLetter;
            });
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
  mounted(){
    this.placeCorrectWords(this.words)
    this.placeCorrectWords(this.tempWords)
    this.generateRandomLetter()

    const cells = document.querySelectorAll('#board .cell-data');
    const board = document.getElementById('board');
    let isDragging = false
    var columnSelected = []
    var rowSelected = []
    var selectedAnswer = ""

    cells.forEach(cell => {
    cell.addEventListener('mousedown', event => {
      if(!event.target.classList.contains('correct')){
        const startRow = parseInt(event.target.getAttribute('data-row'));
        const startCol = parseInt(event.target.getAttribute('data-column'));
        isDragging = true;
        event.target.classList.add('selected')
        selectedAnswer += event.target.innerText
        columnSelected.push(startCol)
        rowSelected.push(startRow)
      }
      
    });

      cell.addEventListener('mouseup', () => {
        cells.forEach(cellDeselect =>{
          cellDeselect.classList.remove('selected');
        });
        isDragging = false;
        if(this.words.some(item => item.toLowerCase() === selectedAnswer.toLowerCase())){
          this.remainingWords-=1;
          for(let i=0; i<columnSelected.length;i++){
            let rowCorrect = rowSelected[i]
            let colCorrect = columnSelected[i]
            const cell = document.querySelector(`.cell-data[data-row="${rowCorrect}"][data-column="${colCorrect}"]`);
            cell.classList.add('correct')
          }
        }
        console.log(selectedAnswer)
        console.log(columnSelected)
        console.log(rowSelected)
        console.log(this.remainingWords)

        selectedAnswer = ""
        columnSelected = []
        rowSelected = []

      });
    });

    board.addEventListener('mouseover', event => {
    if(!event.target.classList.contains('correct')){
      if (isDragging) {
        const currentRow = parseInt(event.target.getAttribute('data-row'));
        const currentCol = parseInt(event.target.getAttribute('data-column'));
        const cell = document.querySelector(`.cell-data[data-row="${currentRow}"][data-column="${currentCol}"]`);
            if (cell) {
              cell.classList.add('selected');
              selectedAnswer += cell.innerText
            }
        columnSelected.push(currentCol)
        rowSelected.push(currentRow)
        } 
      }
     });
  },
  
}
</script>



<style scoped>
  .selected {
    background-color: yellow;
  }

  .correct {
    background-color: rgba(88, 248, 73, 0.507)
  }

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
    padding: 0;
    border: 1px solid black;
    width: 30px;
    height: 30px;
    text-transform: uppercase;
    text-align: center;
    font-size: 12px;
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
  .cell-data {
    user-select: none;
    cursor: pointer;
  }
  #modal-multi-2 h2 {
    text-align: center;
  }
  .modal-content {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 100%;
    pointer-events: auto;
    background-color: #ffffff00 !important;
    background-clip: padding-box;
    border: 1px solid rgba(0, 0, 0, 0.2);
    border-radius: 0.3rem;
    outline: 0;
  }
  button {
    height: 60px;
    border-radius: 15px;
    border: none;
    color: #fff;
    -webkit-text-stroke: .1px #000000;
    transition: 0.3s ease;
    font-weight: 800;
    font-family: 'Coiny', cursive;
    font-size: 2em;
  }
  .button-play {
    padding: 2px 30px 2px 30px;
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
  .modal-window-success {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 999;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transition: all 0.3s;
  }
  .modal-window-success:target {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
    background-color: #00000052;
  }
  .modal-window-success>div {
    color: #fff;
    width: 600px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 2em;
  }
  .modal-window-success h1 {
    text-align: center;
    font-size: 5em;
    margin-bottom: 0;
  }
  .modal-window-success h3 {
    text-align: center;
    font-size: 1.5em;
  }
  .modal-close-success {
    color: #aaa;
    line-height: 50px;
    font-size: 80%;
    position: absolute;
    right: 0;
    text-align: center;
    top: 0;
    width: 70px;
    text-decoration: none;
  }
  .modal-close-success:hover {
    color: black;
  }
  .modal-window-failed {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 999;
    visibility: hidden;
    opacity: 0;
    pointer-events: none;
    transition: all 0.3s;
  }
  .modal-window-failed:target {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
    background-color: #ff0000bd;
  }
  .modal-window-failed>div {
    color: #fff;
    width: 600px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 2em;
  }
  .modal-window-failed h1 {
    text-align: center;
    font-size: 5em;
    margin-bottom: 0;
  }
  .modal-window-failed h3 {
    text-align: center;
    font-size: 1.5em;
  }
  .modal-close-failed {
    color: #aaa;
    line-height: 50px;
    font-size: 80%;
    position: absolute;
    right: 0;
    text-align: center;
    top: 0;
    width: 70px;
    text-decoration: none;
  }
  .modal-close-failed:hover {
    color: black;
  }
  .modal-buttons {
    display: flex;
    justify-content: center;
    gap: 1em;
    width: 100%;
  }
</style>