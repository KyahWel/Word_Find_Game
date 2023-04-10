<template>
    <div class="main-container">
        <transition name="home">
            <MainGame v-if="playInit" :numCols="numCols" :numRows="numRows" :words="words" />
        </transition>
        <div v-if="!playInit" class="menu">
            <h1>WORD FIND</h1>
            <div v-if="!playInit" class="button-class">
                <b-button @click="playInit= true" class="button-play">Play</b-button>
                <b-button v-b-modal.modal-multi-settings class="button-settings">Settings</b-button>
            </div>
            <b-modal id="modal-multi-settings" size="lg" hide-footer no-stacking centered>
                <template #modal-title><h1>SETTINGS</h1>
</template>
                  <p class="my-2">CHOOSE THEME: 
                      <b-button v-b-modal.modal-multi-theme></b-button>
                  </p>
              <b-button class="mt-3 button-show-credits" block v-b-modal.modal-multi-credits><h3>SHOW CREDITS</h3></b-button>
          </b-modal>

          <b-modal id="modal-multi-theme" hide-footer centered>
<template #modal-title>
    <h1>CHOOSE THEME</h1>
</template>
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
<template #modal-title>
    <h1>CREDITS</h1>
</template>
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
                credits: [{
                        name: 'William',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Franco',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Austin',
                        image: 'https://via.placeholder.com/100'
                    },
                ],
                themes: [{
                        name: 'Theme 1',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Theme 2',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Theme 3',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Theme 4',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Theme 5',
                        image: 'https://via.placeholder.com/100'
                    },
                    {
                        name: 'Theme 6',
                        image: 'https://via.placeholder.com/100'
                    },
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
        background-image: radial-gradient(#7DF9FF, #1F30DD);
        box-shadow: inset 0 0 100px hsla(0, 0%, 0%, .3);
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
    .button-settings {
        padding: 2px 30px 2px 30px;
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
        padding: 2px 30px 2px 30px;
        background: linear-gradient(#6d9fb6 25%, #397691 50%);
        border-radius: 10px;
        margin-bottom: 25px;
        box-shadow: 0 10px #6d9fb6;
        -webkit-text-stroke: .8px #000000;
    }
    .button-show-credits:hover {
        box-shadow: 0 8px #6d9fb6;
        transform: translateY(1px);
    }
    .button-show-credits:active {
        box-shadow: 0 5px #6d9fb6;
        transform: translateY(4px);
    }
</style>