<template>
    <div class="main-content-window-overview">
            <h3 class="config-submenu-title">Teams</h3>
            <div class="team-container">
                <div class="overview-list">
                    <div v-for="(team, index) in configs.teamConfigs" :key="team.id" :class="{ selected: index ===  selectedItem}" class="overview-list">
                        <li @click="selectListItem(index)" class="overview-list-item">{{ team.name }}</li>
                    </div>
                </div>
                <div class="overview-options">
                    <button @click="editTeamConfig(selectedItem)" class="small-button overview-options-button">Bearbeiten</button>
                    <button @click="downloadJSON()" class="small-button overview-options-button"><a id="downloadAnchorElem" style="display:none"></a>Download</button>
                    <button @click="deleteTeamConfig(selectedItem)" class="small-button overview-options-button">Löschen</button>
                </div>
            </div>
            
            <div class="overview__general-options">
                <hr class="team-config__content-container-hr">
                <label for="file-import" class="small-button overview__general-options-button">Importieren</label>
                <!-- <button @click="readFile()" class="small-button team-overview__general-options-button">Importieren</button>
                <input type="file" id="importChooser" @change="readFile()"/> -->
                <input type="file" id="file-import" @change="readFile()"/>
                <button @click="createTeamConfig()" class="small-button overview__general-options-button">Team erstellen</button>
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
        downloadJSON() {
            var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.configs.teamConfigs[this.selectedItem]));
            var downloadAnchorNode = document.createElement('a');
            downloadAnchorNode.setAttribute("href",     dataStr);
            downloadAnchorNode.setAttribute("download", this.configs.teamConfigs[this.selectedItem].name.toLowerCase() + ".json");
            document.body.appendChild(downloadAnchorNode); // required for firefox
            downloadAnchorNode.click();
            downloadAnchorNode.remove();
        },
        selectListItem(index) {
            this.selectedItem = index;
        },
        deleteTeamConfig(index) {
            this.configs.teamConfigs.splice(index, 1);
            const parsed = JSON.stringify(this.configs);
            localStorage.setItem('configs', parsed);
        },
        editTeamConfig(index) {
            this.state.index = index;
            this.state.isNew = false;
            this.state.currentState = 'inTeamConfig';
        },
        createTeamConfig() {
            var newConfig = {   
                name: 'Name',
                motto: 'Motto',
                colors: {
                    primary: 'e2e2e2',
                    secondary: 'e2e2e2'
                },
                image: undefined,
                fans: {
                    goblins: 0,
                    trolls: 0,
                    elfs: 0,
                    nifflers: 0
                },
                players: {
                    seeker: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'f'
                    },
                    keeper: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'm'
                    },
                    chaser1: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'm'
                    },
                    chaser2: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'f'
                    },
                    chaser3: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'f'
                    },
                    beater1: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'f'
                    },
                    beater2: {
                        name: 'Name',
                        broom: undefined,
                        sex: 'm'
                    }
                }   
            }
            this.configs.teamConfigs.unshift(newConfig);
            this.state.index = 0;
            this.state.isNew = true;
            this.state.currentState = 'inTeamConfig';
        },
        readFile(){
            var files = document.getElementById("file-import").files;
            var file = files[0];
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
                reader.onload = function(){
                    data = JSON.parse(reader.result);
                    my_vue.configs.teamConfigs.unshift(data);
                    my_vue.storeConfigs();
                }
            }
        },
        storeConfigs() {
            const parsed = JSON.stringify(this.configs);
            localStorage.setItem('configs', parsed);
        }

    }
}
    
</script>

<style>


#file-import {
    display: none;
}

.selected {
    background: #d3c9b1;
}

#importChooser {
    display: none;
}


.team-container {
    display: block;
    padding: 0 10%;
}

.team-config__content-container-hr {
    border: none;
    border-top: 1px solid #d1cb94;
}



</style>
