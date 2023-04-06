<template>
    <div class="main-container">
        <transition name="home">
            <MainGame v-if="playInit" />
        </transition>
        
        <div v-if="!playInit" class="menu">
            <h1>WORD FIND</h1>
            <div v-if="!playInit" class="button-class">
                <b-button @click="playInit= true" pill class="button-design-play">Play</b-button>
                <b-button v-b-modal.modal-settings pill class="button-design-settings">Settings</b-button>
            </div>
            
            <b-modal id="modal-settings" 
                no-close-on-backdrop 
                ok-only
                centered 
                title="Settings">
                <p class="my-4">Settings part</p>
            </b-modal>
        </div>
      
    </div> 

</template>

<script>
export default {
  data(){
    return{
        playInit: false
      }
    },
  methods: {

  }
}
</script>
   

<style scoped>
    .home-enter-active, .home-leave-active { transition: opacity .5s; }
    .home-enter, .home-leave-active { opacity: 0; }
    .main-container{
        display:flex;
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

    .menu h1{
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
    .menu .button-design-play{
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
    .menu .button-design-play:hover{
       
    }

    /* pakiedit ng design */
    .menu .button-design-settings{
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
    .menu .button-design-settings:hover{
       
    }

    .menu .button-design-diff{
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
    .menu .button-design-play:hover{
       
    }

    .menu .difficulty{
        display: flex;
        gap: 1em;
        justify-content: center;
    }

</style>