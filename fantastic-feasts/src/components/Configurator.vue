<template>
        <div>
            <h1 class="subtitle">Konfigurator</h1>
            <div class="main-content-container">
                <div class="text-content-container">
                    <div v-if="state.currentState === 'inTeamOverview' || state.currentState === 'inMatchOverview'" class="button-header-panel">
                        <button @click="state.currentState = 'inTeamOverview'" :class="{ selected: state.currentState === 'inTeamOverview'}" class="config__header-button" id="config__header-button-left">Team</button>
                        <button @click="state.currentState = 'inMatchOverview'" :class="{ selected: state.currentState === 'inMatchOverview'}" class="config__header-button" id="config__header-button-right">Partie</button>
                    </div>
                    <app-team-config v-if="state.currentState === 'inTeamConfig'" :game="game" :state="state" :configs="configs"></app-team-config>
                    <app-match-config v-if="state.currentState === 'inMatchConfig'" :game="game" :state="state" :configs="configs"></app-match-config>
                    <app-team-overview v-if="state.currentState === 'inTeamOverview'" :game="game" :state="state" :configs="configs"></app-team-overview>
                    <app-match-overview v-if="state.currentState === 'inMatchOverview'" :game="game" :state="state" :configs="configs"></app-match-overview>
                </div>
            </div>
        </div>
</template>

<script>
import TeamConfig from './TeamConfigurator.vue';
import TeamOverview from './TeamOverview.vue';
import MatchConfig from './MatchConfigurator.vue';
import MatchOverview from './MatchOverview.vue';

export default {
    data() {
        return {
            state: {
                currentState: 'inTeamOverview',
                index: 0,
                isNew: false
            } 
        }
    },
    props: ['game', 'configs'],
    components: {
        'app-team-config': TeamConfig,
        'app-match-config': MatchConfig,
        'app-team-overview': TeamOverview,
        'app-match-overview': MatchOverview
    }
}
</script>

<style>

.main-content-window-overview {
    position: absolute;
    width: 90%;
    left: 5%;
    top: 10%;
    height: 85%;
}

.main-content-window-editor {
    position: absolute;
    width: 90%;
    left: 5%;
    top: 0;
    height: 85%;
}

.main-content-container {
    display: inline-block;
    position: absolute;
    width: 80%;
    height: 70%;
    left: 10%;
}

.team-config__content-container h3 {
    margin: 10px 0;    
}

.button-header-panel {
    position: absolute;
    width: 90%;
    left: 5%;
    height: 10%;
}

.text-content-container {
    background: radial-gradient(#ffffff, #ebd18a);
    display: inline-block;
    position: absolute;
    width: 90%;
    left: 5%;
    height: 100%;
    border-radius: 5px;
    color: #795a46;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  }

.page-content {
    position: fixed;
    width: 100%;
    height: 100%;
}

.small-button {
    background: #f8f1df;
    color: #8d6951;
    font-family: 'Alice';
    font-size: 1em;
    border: 1px solid #99735a;
    border-radius: .5vh;
}

.small-button:hover {
    background: #f7e9c6;
}

.team-config__content-container h4 {
    margin: 5px 0;
    display: inline-block;
    width: 80px;
    text-align: left;
}

.config__header-button {
    background: #f3e7c0;
    padding: 4px;
    color: #8d6951;
    font-family: 'Alice';
    font-size: 2vh;
    border: 1px solid #99735a;
    position: absolute;
    height: 60%;
    top: 40%;
    width: 10%;
}

.config__header-button:hover {
    background: #faf0d6;
}

.config__header-button:active {
    background: #fcf2da;
    -moz-box-shadow:    inset 0 0 .8vw #00000036;
    -webkit-box-shadow: inset 0 0 .8vw #00000036;
    box-shadow:         inset 0 0 .8vw #00000036;
}

.config__header-button:focus {
    background: #f7e9c6;
    -moz-box-shadow:    inset 0 0 .8vw #00000036;
    -webkit-box-shadow: inset 0 0 .8vw #00000036;
    box-shadow:         inset 0 0 .8vw #00000036;
}

#config__header-button-left {
    left: 40%;
    border-right: none;
    border-radius: .5vh 0 0 .5vh;
}

#config__header-button-right {
    border-radius: 0 .5vh .5vh 0;
    right: 40%;
}

.team-config__content-container input {
    border-radius: 3px;
    border: .5px solid #ebd18a;
    color: #795a46;
    text-align: left;
    font-family: 'Alice';
    padding: 3px;
    vertical-align: center;
}

.team-config__content-container input:hover {
    box-shadow: 0 0 10px 0 rgba(255, 228, 110, 0.534);
}

.team-config__content-container input:active {
    box-shadow: 0 0 3px 0 rgba(185, 141, 59, 0.534);
}

.team-config__content-container input:active,
.team-config__content-container input:focus {
    box-shadow: 0 0 3px 0 rgba(121, 92, 38, 0.734);
}

.team-config__fan-counter {
    width: 30px;
    border-radius: 3px;
    border: .5px solid #ebd18a;
    color: #795a46;
    text-align: right;
    font-family: 'Alice';
}

.team-config__input-wrapping {
    display: inline-block;
    width: 25%;
    padding: 0 15px;
}

.team-config__fan-label {
    width: 40px;
    color: #795a46;
    text-align: left;
    font-weight: bold;
    margin: 5px;
}

.team-config__player-selection {
    width: 100%;
    position: relative;
}

h4.table-header {
    display: inline-block;
    text-align: center;
    width: 25%;
}

.team-config__team-label {
    width: 40px;
    color: #795a46;
    text-align: left;
    font-weight: bold;
    margin: 5px;
}

.team-config__team-name-input {
    border-radius: 3px;
    border: .5px solid #ebd18a;
    color: #795a46;
    text-align: left;
    width: 10vw;
}

.team-config__team-motto-input {
    border-radius: 3px;
    border: .5px solid #ebd18a;
    color: #795a46;
    text-align: left;
    width: 15vw;
}
.team-config__team-color-input {
    border-radius: 3px;
    border: .5px solid #ebd18a;
    color: #795a46;
    text-align: left;
    width: 60px;
}

.team-config__player-label {
    color: #795a46;
    text-align: center;
    width: 12.5%;
    display: inline-block;
}


.config-submenu-title {
    font-size: 3vh;

}

.team-config__player-name-input {
    color: #795a46;
    font-family: 'Alice';
    text-align: left;
    border-radius: 3px;
    width: 100%;
    border: .5px solid #ebd18a;
}

.team-config__player-broom-input {
    color: #795a46;
    font-family: 'Alice';
    text-align: left;
    width: 100%;
    border-radius: 3px;
    border: .5px solid #ebd18a;
}

.selected {
    -moz-box-shadow:    inset 0 0 .8vw #00000036;
    -webkit-box-shadow: inset 0 0 .8vw #00000036;
    box-shadow:         inset 0 0 .8vw #00000036;
    
}


.overview-options-button {
    text-align: center;
    display: auto;
    width: 60%;
    margin: 1vh 1vh;
}

.overview-list-item {
    list-style: none;
    text-align: left;
}

.overview-list-item:hover,
.overview-list-item:focus {
    background: #e0d9c7;
}
.overview-list-item.active {
    background: #d3c9b1;
}

.overview-list li.active {
    background: #d3c9b1;
}

.overview-options {
    display: inline-block;
    width: 40%;
}

.overview-list {
    vertical-align: top;
    display: inline-block;
    width: 60%;
}

.overview__general-options {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 18%;
}

.overview__general-options-button {
    display: inline-block;
    margin: 0 1vw;
    padding: .5vh 2vh;
}

label.small-button {
    height: 3vh;
    display: inline-block;
    vertical-align: center;
    
}

.overview-options-button {
    text-align: center;
    width: 60%;
    margin: 1vh 1vh;
}


</style>
