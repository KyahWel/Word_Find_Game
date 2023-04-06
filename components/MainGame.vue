<template>
  <div class="main-container-game">
    <div class="header">
      <a v-b-modal.modal-multi-game-settings><i class="fa-solid fa-gears"></i></a>
    </div>
    <div class="main-content">
      <div class="grid">
        <div class="left">
          <div class="board">
            <b-row v-for="row in 10" :key="row">
              <b-column v-for="cols in 14" :key="cols" class="col-design">
              </b-column>
            </b-row>
          </div>
        </div>
        <div class="right">
          <h1>{{minutes}}:{{ seconds }}</h1>
        </div>
      </div>
    </div>

    <b-modal id="modal-multi-game-settings" no-close-on-backdrop centered hide-footer hide-header>
      <b-button v-b-modal.modal-multi-game-settings block pill variant="success">
        <h3>CONTINUE</h3>
      </b-button>

      <b-button v-b-modal.modal-multi-2 block pill variant="danger">
        <h3>QUIT GAME</h3>
      </b-button>

    </b-modal>

    <b-modal id="modal-multi-2" no-close-on-backdrop centered hide-footer>
      <h2>Are you sure?</h2>
      <b-button v-b-modal.modal-multi-1 size="sm" block pill variant="danger">
        <h3>QUIT GAME</h3>
      </b-button>
    </b-modal>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        timerCount: 120, //In Seconds
        minutes: 2,
        seconds: "00",
        remainingWords: 10
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
    methods: {}
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
    align-items: center;
    /* border: 1px solid red; */
    width: 100%;
  }
  .row-design {
    border: 1px solid black;
    padding: 15px;
  }
  .col-design {
    border: 1px solid black;
    padding: 15px;
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
  #modal-multi-2 h2{
    text-align: center;
  }
</style>