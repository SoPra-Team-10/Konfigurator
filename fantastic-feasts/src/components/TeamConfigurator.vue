<template>
    <!-- The main area that includes the configuration options -->
    <div class="main-content-window-editor" id="team-configurator">
        <!-- <h2>Team Konfigurator</h2> -->
             <h3>Team</h3>
            <label class="team-config__team-label" for="team-name">Name</label>
            <input
                type="text"
                id="team-name"
                maxlength="40"
                class="team-config__team-name-input"
                v-model.lazy="teamConfig.name">
            <label class="team-config__team-label" for="team-motto">Motto</label>
            <input
                type="text"
                id="team-motto"
                maxlength="80"
                class="team-config__team-motto-input"
                v-model.lazy="teamConfig.motto">
            <label class="team-config__team-label" for="team-color-primary">Hauptfarbe</label>
            <input
                type="text"
                id="team-color-primary"
                maxlength="6"
                class="team-config__team-color-input"
                v-model.lazy="teamConfig.colors.primary">
            <label class="team-config__team-label" for="team-color-secondary">Zweitfarbe</label>
            <input
                type="text"
                id="team-color-secondary"
                maxlength="6"
                class="team-config__team-color-input"
                v-model.lazy="teamConfig.colors.secondary">
            <h3>Spieler</h3>
            <h4 class="table-header"></h4>
            <h4 class="table-header">Name</h4>
            <h4 class="table-header">Besen</h4>
            <h4 class="table-header">Geschlecht</h4>
            <div v-for="(player, index) in teamConfig.players" :key="index +player.sex+player.broom" class="team-config__player-selection">
                <h4 class="player-type-label">{{ mapRole(index) }}</h4>
                <!-- <label 
                    for="player-name"
                    class="team-config__player-label"
                    >Name</label> -->
                <div class="team-config__input-wrapping">
                    <input
                    type="text"
                    maxlength="30"
                    id="player-name"
                    class="team-config__player-name-input"
                    v-model.lazy="player.name">
                </div>
                
                <!-- <label 
                    for="player-broom"
                    class="team-config__player-label">
                    Besen</label> -->
                <div class="team-config__input-wrapping">
                    <select
                        id="player-broom"
                        class="team-config__player-broom-input"
                        v-model="player.broom">
                    <option v-for="broom in brooms" :selected="broom === 'thinderblast'" :key="broom.id">{{ broom }}</option>
                </select>
                </div> 
                <label 
                    for="male"
                    class="team-config__player-label">
                <input
                    type="radio"
                    id="male"
                    value="m"
                    v-model="player.sex"> m
                </label>
                <label 
                    for="female"
                    class="team-config__player-label">
                <input
                    type="radio"
                    id="female"
                    value="f"
                    v-model="player.sex"> w
                </label>
            </div>
            <!-- The fan options -->
            <h3>Fans (<span :class="[{valid: fanSum == 7},{invalid: fanSum > 7}]">{{ fanSum }}</span> / 7)</h3>
            <div class="fan-selection">
                <label class="team-config__fan-label" for="team-goblins">Goblins</label>
                <input
                    type="number"
                    id="team-goblins"
                    min='1'
                    max='3'
                    class="team-config__fan-counter"
                    v-model="teamConfig.fans.goblins">
                <label class="team-config__fan-label" for="team-trolls">Trolle</label>
                <input
                    type="number"
                    id="team-trolls"
                    min='1'
                    max='3'
                    class="team-config__fan-counter"
                    v-model="teamConfig.fans.trolls">
                <label class="team-config__fan-label" for="team-elfs">Elfen</label>
                <input
                    type="number"
                    id="team-elfs"
                    min='1'
                    max='3'
                    class="team-config__fan-counter"
                    v-model="teamConfig.fans.elfs">
                <label class="team-config__fan-label" for="team-nifflers">Niffler</label>
                <input
                    type="number"
                    id="team-nifflers"
                    min='1'
                    max='3'
                    class="team-config__fan-counter"
                    v-model="teamConfig.fans.nifflers">
                <!-- Buttons at the bottom -->
                <div class="main-menu__button-container">
                    <button @click="saveTeamConfig()" class="main-menu__small-button">Speichern</button>
                    <button @click="discardChanges()" class="main-menu__small-button">Verwerfen</button>
                </div>
            </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            //define viable options
            playerRoles: [
                'Sucher', 'Hüter', 'Jäger', 'Jäger', 'Jäger', 'Treiber', 'Treiber'
            ],
            brooms: [
                'thinderblast', 'cleansweep-11', 'comet-260', 'nimbus-2001', 'firebolt'
            ],
            teamConfig: this.configs.teamConfigs[this.state.index],
            status: '',
            alertMessage: ''
        }
    },
    computed: {
        //returns the total number of fans
        fanSum() {
            var sum = 0;
            for(var key in this.teamConfig.fans) {
                sum += parseInt(this.teamConfig.fans[key], 10);
            }
            return sum;
        }
    }, 
    methods: {
        //Translates role names to German for display
        mapRole(index) {
            switch (index) {
                case 'seeker':
                    return 'Sucher';
                case 'keeper':
                    return 'Hüter';
                case 'chaser1':
                    return 'Jäger 1';
                case 'chaser2':
                    return 'Jäger 2';
                case 'chaser3':
                    return 'Jäger 3';
                case 'beater1':
                    return 'Treiber 1';
                case 'beater2':
                    return 'Treiber 2';
                default: 
                return undefined;
            }
        },
        //Stores the current Configuration into the browser cache
        saveTeamConfig() {
            const teamConfig = this.teamConfig;
            if(this.validateTeamConfig(teamConfig)) {
                this.storeTeamConfigs();
                this.state.currentState = 'inTeamOverview';
            } else {
                alert("Keine gültige Konfiguration.");
                this.status = 'notValid';
            }
            
        },
        validateTeamConfig(config) {
            console.log(config);
            if(config.name.length === 0) {
                console.log('Name');
                return false;
            }
            if(config.motto.length === 0) {
                console.log('Motto');
                return false;
            }
            if(config.colors.primary.length === 0) {
                console.log('primary');
                return false;
            }
            if(config.colors.secondary.length === 0) {
                console.log('secondary');
                return false;
            }
            const fans = config.fans;
            let fanSum = 0;
            for(var key in fans) {
                fanSum += parseInt(fans[key]);
            }
            console.log(fanSum);
            if(fanSum !== 7) {
                console.log('fans');
                return false;
            }
            const players = config.players;
            for(var key in players) {
                if(players[key].name === "") {
                    console.log('player-name');
                    return false;
                }
                if(players[key].broom === "") {
                    console.log('player-broom');
                    return false;
                }
            }
            return true;
        },
        discardChanges() {
            if (this.state.isNew) {
                this.configs.teamConfigs.splice(0, 1);
                this.state.currentState = 'inTeamOverview';
            } else {
                if (localStorage.getItem('configs')) {
                    try {
                        this.configs.teamConfigs = JSON.parse(localStorage.getItem('configs')).teamConfigs;
                        this.state.currentState = 'inTeamOverview';
                    } catch(e) {
                        localStorage.removeItem('configs');
                        this.configs.teamConfigs = null;
                        this.state.currentState = 'inTeamOverview';
                    }             
                }
            }
        },
        storeTeamConfigs() {
            const parsed = JSON.stringify(this.configs);
            localStorage.setItem('configs', parsed);
        }

    },
    props: ['configs', 'state']
}
    
</script>

<style>
.team-config__player-selection {
    margin: 0;
    padding: 0;
}

#team-configurator input {
    padding: 0.5vh;
    font-size: 1.5vh;
    font-family: 'Alice';
}

#team-configurator option {
    padding: 0.5vh;
    font-size: 1.4vh;
    font-family: 'Alice';
}

.valid {
    color: #54bd62;
}

.invalid {
    color: #b84747;
}

.player-type-label {
    display: inline-block;
    width: 25%;
    margin-block-start: 0;
}


</style>
