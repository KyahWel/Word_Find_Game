<template>
  <div class="main-container">
      <transition name="home">
          <MainGame v-if="playInit" :numCols="numCols" :numRows="numRows" :words="words"/>
      </transition>
      <div v-if="!playInit" class="menu">
          <h1>WORD FIND</h1>
          <div v-if="!playInit" class="button-class">
              <b-button @click="playInit= true" pill class="button-design-play">Play</b-button>
              <b-button v-b-modal.modal-multi-settings pill class="button-design-settings">Settings</b-button>
          </div>
      
          <b-modal id="modal-multi-settings" size="lg" hide-footer no-stacking centered>
              <template #modal-title><h1>SETTINGS</h1></template>
                  <p class="my-2">CHOOSE THEME: 
                      <b-button v-b-modal.modal-multi-theme></b-button>
                  </p>
              <b-button class="mt-3" block pill v-b-modal.modal-multi-credits><h3>SHOW CREDITS</h3></b-button>
          </b-modal>

          <b-modal id="modal-multi-theme" hide-footer centered>
              <template #modal-title><h1>CHOOSE THEME</h1></template>
              <b-container fluid>
                  <b-row>
                  <b-col v-for="(item, index) in themes" :key="index" cols="12" sm="6" md="4" class="mb-3">
                      <img :src="item.image" :alt="item.name" class="img-fluid mb-2" width="100" height="100">
                      <p class="mb-0">{{ item.name }}</p>
                  </b-col>
                  </b-row>
              </b-container>
          </b-modal>

          <b-modal id="modal-multi-credits" hide-footer centered>
              <template #modal-title><h1>CREDITS</h1></template>
              <b-container fluid>
                  <b-row>
                  <b-col v-for="(item, index) in credits" :key="index" cols="12" sm="6" md="4" class="mb-3">
                      <img :src="item.image" :alt="item.name" class="img-fluid mb-2" width="100" height="100">
                      <p class="mb-0">{{ item.name }}</p>
                  </b-col>
                  </b-row>
              </b-container>
          </b-modal>
      </div>
  </div> 

</template>

<script>
  export default {
      data() {
          return {
              playInit: false,
              numCols: 14,
              numRows: 12,
              words: ['apple', 'banana', 'cherry', 'date', 'elderberry', 'fig', 'grape', 'honeydew', 'kiwi', 'lemon'],
              credits: [
                  { name: 'William', image: 'https://via.placeholder.com/100' },
                  { name: 'Franco', image: 'https://via.placeholder.com/100' },
                  { name: 'Austin', image: 'https://via.placeholder.com/100' },
              ],
              themes: [
                  { name: 'Theme 1', image: 'https://via.placeholder.com/100' },
                  { name: 'Theme 2', image: 'https://via.placeholder.com/100' },
                  { name: 'Theme 3', image: 'https://via.placeholder.com/100' },
                  { name: 'Theme 4', image: 'https://via.placeholder.com/100' },
                  { name: 'Theme 5', image: 'https://via.placeholder.com/100' },
                  { name: 'Theme 6', image: 'https://via.placeholder.com/100' },
              ],
          }
      },
      methods: {}
  }
</script>
 

<style scoped>
  .home-enter-active,
  .home-leave-active {
      transition: opacity .5s;
  }
  .home-enter,
  .home-leave-active {
      opacity: 0;
  }
  .main-container {
      display: flex;
      flex-direction: column;
      min-height: 100vh;
      width: 100%;
      justify-content: center;
      align-items: center;
      padding: 0;
      margin: 0;
      background-image: url("~/static/background-1.png");
      background-position: center;
      background-repeat: no-repeat;
      background-size: cover;
  }
  .menu h1 {
      font-family: 'Bangers', cursive;
      font-size: 190px;
      color: white;
      text-shadow: 2px 2px 4px #000000;
  }
  .menu .button-class {
      display: flex;
      justify-content: center;
      gap: 1em;
      width: 100%;
  }
  /* pakiedit ng design */
  .menu .button-design-play {
      background-color: rgb(73, 248, 73);
      font-family: 'Coiny', cursive;
      font-weight: 800;
      font-size: 2em;
      width: 20%;
      padding: 8px 2px;
      text-shadow: 2px 2px 4px #000000;
      box-shadow: 2px 2px 4px #000000;
  }
  /* pakiedit ng design */
  .menu .button-design-play:hover {}
  /* pakiedit ng design */
  .menu .button-design-settings {
      background-color: #397691;
      font-family: 'Coiny', cursive;
      font-weight: 800;
      border: 2px solid #397691;
      font-size: 2em;
      padding: 8px 2px;
      width: 30%;
      text-shadow: 2px 2px 4px #000000;
      box-shadow: 2px 2px 4px #000000;
  }
  /* pakiedit ng design */
  .menu .button-design-settings:hover {}
  .menu .button-design-diff {
      background-color: rgb(73, 248, 73);
      font-family: 'Coiny', cursive;
      font-weight: 800;
      font-size: 2em;
      width: 20%;
      padding: 8px 2px;
      text-shadow: 2px 2px 4px #000000;
      box-shadow: 2px 2px 4px #000000;
  }
  /* pakiedit ng design */
  .menu .button-design-play:hover {}
  .menu .difficulty {
      display: flex;
      gap: 1em;
      justify-content: center;
  }
  .modal-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  padding: 1rem 1rem;
  border-bottom: none !important;
  border-top-left-radius: calc(0.3rem - 1px);
  border-top-right-radius: calc(0.3rem - 1px);
  }
</style>