<template>
    <div class="main-container-game">
      <div class="header">

        <a v-b-modal.modal-multi-game-settings><i class="fa-solid fa-gears"></i></a>
        
      </div>
      <div class="main-content">
        <div class="grid">
        <div class="left"> 
           <div class="board" id="board">
              <b-row v-for="row in numRows" :key="row">
                  <b-col v-for="cols in numCols" 
                    :key="cols"
                    :data-row="row"
                    :data-column="cols"
                    class="col-design cell-data">
                   
                  </b-col>
              </b-row>
           </div>
        </div>
        <div class="right">
          <h1>{{minutes}}:{{ seconds }}</h1>
          <b-container>
            <b-row class="text-center">
              <b-col v-for="word in words" :key="word" cols="4" >
                {{word}}
              </b-col>
            </b-row>
          </b-container>
        </div>
      </div>
      <b-modal id="modal-multi-game-settings"  no-close-on-backdrop centered hide-footer hide-header>
          <template #default="{ close }">
            <b-button @click="close()" block pill variant="success">
              <h3>CONTINUE</h3>
            </b-button>
          
          <b-button v-b-modal.modal-multi-2 block pill variant="danger">
            <h3>QUIT GAME</h3>
          </b-button>
        </template>
        </b-modal>

        <b-modal id="modal-multi-2" no-close-on-backdrop centered hide-footer>
          <h2>Are you sure?</h2>
          <b-button v-b-modal.modal-multi-1 size="sm" block pill variant="danger">
            <h3>QUIT GAME</h3>
          </b-button>
        </b-modal>
    </div>
  </div>
</template>

<script>
export default {

  props:['words'],
  data(){
    return{
      numRows: this.words.length * 2,
      numCols: this.words.length * 2 ,
      timerCount: 120, //In Seconds
      minutes: 2,
      seconds: "00",
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
  methods:{
    checkCellOccupied(word, startPosition, orientation){
        var status = ''
        var incrementBy = 0 
        const individuals = document.querySelectorAll('.cell-data');
        if(orientation === 'row'){
          incrementBy = 1
        }
        else if(orientation === 'column'){
          incrementBy = this.numCols
        }
        else if(orientation === 'diagonal'){
          incrementBy = this.numCols + 1
        }
        for(let p = startPosition,q=0; q<word.length; q++){
          if(individuals[p].hasAttribute('data-word')){
            status = 'occupied'
            break
          }
          else{
            status = 'empty'
          }
          p+= incrementBy
        }
        return status
    },
    
    placeCorrectWords(wordsArray){
      var positions = ['row', 'column','diagonal']
      var nextLetter = 0
      var newStartPoint = 0
      for(let i = 0 ; i<wordsArray.length; i++){
        var getOrientation = positions[Math.floor(Math.random()*positions.length)] 
        const individuals = document.querySelectorAll('.cell-data');
        const start = Math.floor(Math.random()*individuals.length)
        
        const startRow = individuals[start].getAttribute('data-row');
        
        const startColumn = individuals[start].getAttribute('data-column');
    
        if(getOrientation === 'row'){
          nextLetter = 1 
          if((startColumn*1)+ wordsArray[i].length <= this.numCols){
            newStartPoint = start
          }
          else{
            var newColumnPoint = this.numCols - wordsArray[i].length - 1
            //New row Starting Point
            newStartPoint = ((startRow-1)*this.numCols)+(newColumnPoint-1)
          }
        }
        else if(getOrientation === 'column'){
          nextLetter = this.numCols
          if((startRow*1)+ wordsArray[i].length <= this.numRows){
            newStartPoint = start
          }
          else{
            var newRowPoint = this.numRows - wordsArray[i].length - 1
            //New column Starting Point
            newStartPoint = newRowPoint * startColumn
          }
        }
        else if(getOrientation === 'diagonal'){
          nextLetter = 1 + this.numCols
          if((startColumn*1)+ wordsArray[i].length <= this.numCols && 
          (startRow*1)+ wordsArray[i].length <= this.numRows){
            newStartPoint = start
          }
          //Column
          else if((startColumn*1)+ wordsArray[i].length > this.numCols &&
          (startRow*1)+ wordsArray[i].length <= this.numRows ){
            var newRowStartPoint= this.numCols - (wordsArray[i].length-1)
            //New row Starting Point
            newStartPoint = (newRowStartPoint * this.numRows) - (this.numRows - startRow)
            console.log(`[1] Word: ${wordsArray[i]}, New Start: ${newStartPoint}, New Row: ${startRow}, Column: ${newColStartPoint}, Orientation: ${getOrientation}`)
          }
           //Row
           else if((startColumn*1)+ wordsArray[i].length <= this.numCols && 
           (startRow*1)+ wordsArray[i].length > this.numRows ){
            var newColStartPoint = this.numRows - (wordsArray[i].length-1)
            //New column Starting Point
            newStartPoint = ((newColStartPoint* this.numCols) - (this.numCols - startColumn) - 1)
            console.log(`[2] Word: ${wordsArray[i]}, New Start: ${newStartPoint}, New Row: ${newRowStartPoint}, Column: ${startColumn}, Orientation: ${getOrientation}`)
          }
          else if((startColumn*1)+ wordsArray[i].length > this.numCols && 
          (startRow*1)+ wordsArray[i].length > this.numRows){
            var newColStartPoint = this.numRows - (wordsArray[i].length-1)
            var newRowStartPoint= this.numCols - (wordsArray[i].length-1)
            //Fix this shit
            newStartPoint =((newRowStartPoint * this.numRows) - (this.numRows - startRow)) - this.numCols 
            console.log(`[3] Word: ${wordsArray[i]}, New Start: ${newStartPoint}, New Row: ${newRowStartPoint}, New Column: ${newColStartPoint}, Orientation: ${getOrientation}`)
          }
        }

        var characters = wordsArray[i].split("");
        var nextPosition = 0
        var isCellOccupied = this.checkCellOccupied(wordsArray[i], newStartPoint, getOrientation)
        if(isCellOccupied === 'empty'){
            characters.forEach(item => {
            individuals[newStartPoint + nextPosition].innerText = item
            individuals[newStartPoint + nextPosition].setAttribute('data-word', wordsArray[i])
            individuals[newStartPoint + nextPosition].style.backgroundColor = "rgba(88, 248, 73, 0.507)";
            nextPosition += nextLetter;
          });
        }
        else{
          this.tempWords.push(wordsArray[i])
        }
      }
    },

    generateRandomLetter(){
      const individuals = document.querySelectorAll('.cell-data');
      var alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
      for(let i=0; i<individuals.length;i++){
        if(!individuals[i].hasAttribute('data-word')){
            individuals[i].innerText = alphabets.charAt(Math.floor(Math.random()*26))
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
    let isDragging = false;
    let startRow, startCol;

    cells.forEach(cell => {
    cell.addEventListener('mousedown', event => {
      isDragging = true;
      startRow = parseInt(event.target.getAttribute('data-row'));
      startCol = parseInt(event.target.getAttribute('data-column'));
      event.target.classList.add('selected');
    });

      cell.addEventListener('mouseup', () => {
        isDragging = false;
      });
    });

    board.addEventListener('mouseover', event => {
    if (isDragging) {
      const currentRow = parseInt(event.target.getAttribute('data-row'));
      const currentCol = parseInt(event.target.getAttribute('data-column'));

      if (currentCol === startCol) {
        const minRow = Math.min(startRow, currentRow);
        const maxRow = Math.max(startRow, currentRow);

        for (let i = minRow; i <= maxRow; i++) {
          const cell = document.querySelector(`.cell-data[data-row="${i}"][data-column="${startCol}"]`);
          if (cell) {
            cell.classList.add('selected');
          }
        }
      }
      else if (Math.abs(currentCol - startCol) === Math.abs(currentRow - startRow)) {
        const minRow = Math.min(startRow, currentRow);
        const maxRow = Math.max(startRow, currentRow);
        const minCol = Math.min(startCol, currentCol);
        const maxCol = Math.max(startCol, currentCol);

        for (let i = minRow, j = minCol; i <= maxRow && j <= maxCol; i++, j++) {
          const cell = document.querySelector(`.cell-data[data-row="${i}"][data-column="${j}"]`);
          if (cell) {
            cell.classList.add('selected');
          }
        }
      }
      else {
        const minCol = Math.min(startCol, currentCol);
        const maxCol = Math.max(startCol, currentCol);

        for (let i = minCol; i <= maxCol; i++) {
          const cell = document.querySelector(`.cell-data[data-row="${startRow}"][data-column="${i}"]`);
          if (cell) {
            cell.classList.add('selected');
              }
            }
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
    width: 25px;
    height: 25px;
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

  #modal-multi-2 h2{
    text-align: center;
  }
</style>