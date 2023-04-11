<template>
    <div class="main-container">
        <transition name="home">
            <MainGame v-if="playInit" :words="words" :time="time" />
        </transition>
        <div v-if="!playInit" class="menu">
            <h1>WORD FIND</h1>
            <div v-if="!playInit" class="button-class">
                <b-button @click="playInit= true" class="button-play">Play</b-button>
                <b-button v-b-modal.modal-multi-credits class="button-credits">Credits</b-button>
            </div>
          <b-modal id="modal-multi-credits" hide-footer centered>
            <template #modal-title>
                <h1>CREDITS</h1>
            </template>
              <b-container fluid>
                  <b-row>
                  <b-col v-for="(item, index) in credits" :key="index" cols="12" sm="6" md="4" class="mb-3">
                      <img :src="item.image" :alt="item.name" class="img-fluid mb-2" width="120" height="120">
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
              time: 180, //in seconds
              words: ['test', 'pants', 'banana', 'cherry', 'date', 'fig', 'grape', 'honeydew', 'kiwi', 'lemon'],
              credits: [
                  { name: 'William', image: 'https://scontent.fmnl17-3.fna.fbcdn.net/v/t39.30808-6/336363750_220454573815237_7365260229457514060_n.jpg?_nc_cat=106&ccb=1-7&_nc_sid=09cbfe&_nc_eui2=AeFYMTWeFUT6nkf-dK80CFqxCiCQWJkVXcwKIJBYmRVdzNRI0Ef9JAZtKeB8z-l4WMXetOIPqAA05EqqDyLS6_Xs&_nc_ohc=gXswci9e-igAX_sESip&_nc_ht=scontent.fmnl17-3.fna&oh=00_AfC1ig3OMvh8w4A8gnN1L_p53YoDiTHfYl0ZnRhcnuS2sg&oe=6439E57F' },
                  { name: 'Franco', image: 'https://scontent.fmnl17-5.fna.fbcdn.net/v/t39.30808-1/311998514_2533906706752320_799316179013250804_n.jpg?stp=c0.25.160.160a_dst-jpg_p160x160&_nc_cat=105&ccb=1-7&_nc_sid=7206a8&_nc_eui2=AeFoQodgFGEdlShwKvzojp2fhggm0m5coLKGCCbSblygsnsptsuY1kJazoVox4pFQM_xUhxJH-x4pDybsQOQ3Nsz&_nc_ohc=PZXmiVoi52UAX-sD6G3&_nc_ht=scontent.fmnl17-5.fna&oh=00_AfB-5HhwztK0kx3kl0OZsNENgOAELs1CH4PNL2vprUlmsg&oe=6439C342' },
                  { name: 'Austin', image: 'https://scontent.fmnl17-1.fna.fbcdn.net/v/t39.30808-6/329211306_1232696774331423_6929020937128405754_n.jpg?stp=cp6_dst-jpg&_nc_cat=100&ccb=1-7&_nc_sid=09cbfe&_nc_eui2=AeFmUUoLzcYVkogEPFkLqQH3JfQxC_Uqbbwl9DEL9SptvFFEwSc1LoaKZxvfCfnQlZxSZ4goNavimWBVcLs8snZx&_nc_ohc=_Oa-ka3qumgAX8JZw_Y&_nc_ht=scontent.fmnl17-1.fna&oh=00_AfC5qa6Q2qe_sL24Nvhnf_nZW_sMkhFsQ9HF12REkZOPhw&oe=643B3E5D' },
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
        box-shadow: inset 0 0 100px hsla(0,0%,0%,.3);
    }
    .menu h1 {
        font-family: 'Bangers', cursive;
        font-size: 190px;
        color: rgb(255, 255, 255);
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
        background-color: transparent !important;
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
    .button-credits {
        padding: 2px 30px 2px 30px;
        background: linear-gradient(#6d9fb6 25%, #397691 50%);
        border-radius: 10px;
        margin-bottom: 25px;
        box-shadow: 0 10px #6d9fb6;
        -webkit-text-stroke: .4px #000000;
    }
    .button-credits:hover {
        box-shadow: 0 8px #6d9fb6;
        transform: translateY(1px);
    }
    .button-credits:active {
        box-shadow: 0 5px #6d9fb6;
        transform: translateY(4px);
    }

    @media screen and (max-width: 769px){
    .menu h1 {
      font-size: 100px;
     }
     button {
        height: 60px;
        border-radius: 15px;
        font-weight: 800;

        font-size: 1.5em;
        }
        
    }

    @media screen and (max-width: 767px){
        .img-fluid{
            width: 200px !important;
            height: 200px  !important;
        }

        .name{
            font-size: 30px !important;
        }

    }

    @media screen and (max-width: 376px){
    .menu{
        text-align: center;
    }
    .menu h1 {
      font-size: 60px;
     }
     button {
        height: 40px;
        border-radius: 15px;
        font-weight: 800;
        -webkit-text-stroke: .1px #000000;
        font-size: 1em;
        }
         .button-credits {
        -webkit-text-stroke: .1px #000000;
        }
    }
   
    
</style>