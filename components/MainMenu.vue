<template>
    <div class="main-container">
        <h1>WORD FIND</h1>
        <div class="button-class">
            <b-button v-b-modal.modal-play pill class="button-design-play">Play</b-button>
            <b-button v-b-modal.modal-settings pill class="button-design-settings">Settings</b-button>
        </div>

        <b-modal id="modal-play" 
            no-close-on-backdrop 
            centered 
            ok-only
            title="Select Difficulty">
            <p class="my-4">This is difficulty selection part</p>
        </b-modal>

        <b-modal id="modal-settings" 
            no-close-on-backdrop 
            ok-only
            centered 
            title="Settings">
            <p class="my-4">Settings part</p>
        </b-modal>
    </div> 

</template>

<script>
export default {
  data(){
    return{

      }
    },
  methods: {

  }
}
</script>
   

<style scoped>
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

    h1{
        font-family: 'Bangers', cursive;
        font-size: 190px;
        color: white;
        text-shadow: 2px 2px 4px #000000;
    }

    .button-class{
        display: flex;
        justify-content: center;
        gap: 1em;
        width: 100%;
    }

    /* pakiedit ng design */
    .button-design-play{
        background-color: rgb(73, 248, 73);
        font-family: 'Coiny', cursive;
        font-weight: 800;
        background-color: rgb(73, 248, 73);
        font-size: 2em;
        width: 20%;
        padding: 8px 2px;
        text-shadow: 2px 2px 4px #000000;
        box-shadow: 2px 2px 4px #000000;
    }

    /* pakiedit ng design */
    .button-design-play:hover{
       
    }

    /* pakiedit ng design */
    .button-design-settings{
        background-color: #397691;
        font-family: 'Coiny', cursive;
        font-weight: 800;
        border: 2px solid #397691;
        font-size: 2em;
        padding: 8px 2px;
        width: 20%;
        text-shadow: 2px 2px 4px #000000;
        box-shadow: 2px 2px 4px #000000;
    }

    /* pakiedit ng design */
    .button-design-settings:hover{
       
    }

</style>