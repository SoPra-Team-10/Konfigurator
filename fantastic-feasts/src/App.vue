<template>
    <app-config class="menu" :class="[game.colorSchemes[game.selectedColorScheme]]" v-if="game.currentState === 'inConfig'" :game="game" :configs="configs"></app-config>
</template>

<script>

import Config from './components/Configurator.vue';

export default {
  name: 'app',
  data () {
    return {
      game: {
        currentState: 'inConfig',
        colorSchemes: ['griffindor-color-scheme', 'slytherin-color-scheme', 'hufflepuff-color-scheme', 'ravenclaw-color-scheme'],
        selectedColorScheme: 0,
        userName: ""
      },
      
      configs: {
        matchConfigs: [],
        teamConfigs: [],
        selectedTeam: undefined
      }
    }
  },
  components: {
    'app-config': Config
  },
  mounted() {
    if (localStorage.getItem('configs')) {
      try {
        this.configs = JSON.parse(localStorage.getItem('configs'));
      } catch(e) {
        localStorage.removeItem('configs');
      }
    }
  },
  
}
</script>

<style>

  html {
    margin: 0;
  }

  body {
    margin: 0;
    display: fixed; 
  }

  * {
    box-sizing: border-box;
  }

  .menu {
    position: fixed;
    width: 100%;
    height: 100%;
    text-align: center;
  }

  .griffindor-color-scheme {
    background: radial-gradient(#bb3434, #751c1c);
  }

  .slytherin-color-scheme {
    background: radial-gradient(#24867b, #07584e);
  }

  .hufflepuff-color-scheme {
    background: radial-gradient(#2657b1, #09317a);
  }

  .ravenclaw-color-scheme {
    background: radial-gradient(#6744a8, #380f68);
  }

  #main-menu {
    text-align: center;
  }

  hr {
    border: 1px solid #e0a500;
    margin: 30px 0 30px 0;
  }

  .main-menu__button {
    background: #502d16;
    display: auto;
    color: #e0a500;
    font-family: 'Alice';
    font-size: 2.3vh;
    border: 1px solid #e0a500;
    border-radius: 4px;
    margin: 1.8vh;
    width: 18vw;
    height: 4vh;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 3px 10px 0 rgba(0, 0, 0, 0.19);
  }

  .main-menu__button:hover {
    box-shadow: 0 0 10px 0 rgba(255, 228, 110, 0.534);
    color: #f8c22d;
  }
  .main-menu__button:focus {
    background: #5f361a;
    box-shadow: 0 0 10px 0 rgba(255, 228, 110, 0.534);
    color: #f8c22d;
  }

  textarea:focus, input:focus{
    outline: none;
  }

  button:focus{
    outline: none;
  }

  .main-title {
    font-family: 'Alice';
    color: #e0a80c;
    font-size: 10vh;
    text-shadow: 1px 1px 2px #1b1b1ba6;
    /* text-shadow: 0px 0px 3px #FFD938; */
  }

  .subtitle {
    font-family: 'Alice';
    color: #f1c534;
    font-size: 6vh;
    text-shadow: 1px 1px 2px #1b1b1ba6;
  }

  

  .menu__input {
    margin: 10px;
    height: 30px;
    color: #6e6e6e;
    font-family: 'Alice';
    font-size: 1.5em;
  }

  .menu__input-label {
    color: #795a46;
    margin: 5px;
    font-size: 1.5em;
  }

  .help__text {
    text-align: left;
  }

  #port {
    margin-left: 34px;
  }

  #file-chooser {
    padding: 2px 4px;
  }

  .main-menu__small-button {
    background: #f8f1df;
    display: auto;
    color: #8d6951;
    font-family: 'Alice';
    font-size: 1em;
    border: 1px solid #99735a;
    border-radius: .5vh;
    margin: 10px 5px 0 5px;
  }

  .main-menu__small-button:hover {
    background: #f7e9c6;
  }

  #lobby-description {
    color: #795a46;
    font-size: 1.5em;
  }

  .footer-bar {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 20vh;
  }

</style>
