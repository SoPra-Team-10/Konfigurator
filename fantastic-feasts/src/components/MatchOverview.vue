<template>
    <div class="main-content-window-overview">
            <h3 class="config-submenu-title">Partiekonfigurationen</h3>
            <div class="team-container">
                <div class="overview-list">
                    <!-- List of selectable configurations -->
                    <li v-for="(match, index) in configs.matchConfigs" :key="match.id" @click="selectListItem(index)" class="overview-list-item" :class="{ 'selected-list-item': index ===  selectedItem}">{{ match.name }}
                    </li>
                </div>
                <!-- The buttons on the right -->
                <div class="overview-options">
                    <button @click="editConfig(selectedItem)" class="small-button overview-options-button">Bearbeiten</button>
                    <button @click="downloadJSON()" class="small-button overview-options-button"><a id="downloadAnchorElem" style="display:none"></a>Download</button>
                    <button @click="deleteConfig(selectedItem)" class="small-button overview-options-button">Löschen</button>
                </div>
            </div>
            
            <!-- The buttons at the bottom -->
            <div class="overview__general-options">
                <hr class="team-config__content-container-hr">
                <label for="file-import" class="small-button overview__general-options-button">Importieren</label>
                <!-- <button @click="readFile()" class="small-button team-overview__general-options-button">Importieren</button>
                <input type="file" id="importChooser" @change="readFile()"/> -->
                <input type="file" id="file-import" @change="readFile()"/>
                <button @click="createMatchConfig()" class="small-button overview__general-options-button">Partiekonfiguration erstellen</button>
            </div>
    </div>
</template>

<script>
export default {
    props: ['configs', 'state'],
    data() {
        return {
            selectedItem: 0
        }   
    },
    methods: {
        //Downloads the configuration as a json file, writing it to the hard drive
        downloadJSON() {
            var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.configs.matchConfigs[this.selectedItem].config));
            var downloadAnchorNode = document.createElement('a');
            downloadAnchorNode.setAttribute("href",     dataStr);
            downloadAnchorNode.setAttribute("download", this.configs.matchConfigs[this.selectedItem].name.toLowerCase() + ".json");
            document.body.appendChild(downloadAnchorNode); // required for firefox
            downloadAnchorNode.click();
            downloadAnchorNode.remove();
        },
        selectListItem(index) {
            this.selectedItem = index;
        },
        //Removes selected configuration from the cache
        deleteConfig(index) {
            this.configs.matchConfigs.splice(index, 1);
            const parsed = JSON.stringify(this.configs);
            localStorage.setItem('configs', parsed);
        },
        //Switches to the MatchConfigurator component and loads the selected configuration
        editConfig(index) {
            console.log(index);
            console.log(this.configs.matchConfigs[index]);
            this.state.index = index;
            this.state.isNew = false;
            this.state.currentState = 'inMatchConfig';
        },
        //Sets up a new configuration and switches to the MatchConfigurator component. All values are 
        //initialized with 0.
        createMatchConfig() {
            var newConfig = {   
                maxRounds: 0,
                timeouts: {
                    playerTurnTimeout: 0,
                    fanTurnTimeout: 0,
                    playerPhaseTime: 0,
                    fanPhaseTime: 0,
                    ballPhaseTime: 0
                },
                probabilities: {
                    goal: 0.0,
                    throwSuccess: 0.0,
                    knockOut: 0.0,
                    foolAway: 0.0,
                    catchSnitch: 0.0,
                    catchQuaffle: 0.0,
                    wrestQuaffle: 0.0,
                    extraMove: {
                        thinderblast: 0.0,
                        cleansweep11: 0.0,
                        comet260: 0.0,
                        nimbus2001: 0.0,
                        firebolt: 0.0
                    },
                    foulDetection: {
                        flacking: 0.0,
                        haversacking: 0.0,
                        stooging: 0.0,
                        blatching: 0.0,
                        snitchnip: 0.0
                    },
                    fanFoulDetection: {
                        elfTeleportation: 0.0,
                        goblinShock: 0.0,
                        trollRoar: 0.0,
                        snitchSnatch: 0.0
                    }
                }   
            }
            var newEntry = {
                name: 'new_match_config',
                config: newConfig
            }
            this.configs.matchConfigs.unshift(newEntry);
            this.state.index = 0;
            this.state.isNew = true;
            this.state.currentState = 'inMatchConfig';
        },
        storeConfigs() {
            const parsed = JSON.stringify(this.configs);
            console.log(parsed);
            localStorage.setItem('configs', parsed);
        },
        //Reads a json file on the hard drive and conerts it to a javascript object
        readFile(){
            var files = document.getElementById("file-import").files;
            var file = files[0];
            var name = file.name;
            //Check of the selected file is a single json file
            if(files.length !== 1){
                alert("Please choose one file only");
            }
            else if(file.type !== "application/json"){
                alert("Please choose a json-file");
            }
            else{
                
                var reader = new FileReader();
                reader.readAsText(file);
                var data;
                var my_vue = this;
                //The reading process is asynchron
                reader.onload = function(){
                    data = JSON.parse(reader.result);
                    console.log(data);
                    alert(name);
                    var newEntry = {
                        name: name,
                        config: data
                    };
                    console.log(newEntry);
                    my_vue.configs.matchConfigs.unshift(newEntry);
                    my_vue.storeConfigs();
                }
            }
        }
    }
}
    
</script>

<style scoped>


#file-import {
    display: none;
}

#importChooser {
    display: none;
}

</style>
