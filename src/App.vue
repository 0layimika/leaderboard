<template>
  <div style="width:605px; margin:auto; text-align: center; font: bold;">
    <ag-grid-vue :rowData="rowData" :columnDefs="colDefs" style="height:600px" class="ag-theme-quartz"
      :rowheight="rowheight">
    </ag-grid-vue>
  </div>

  <el-dialog v-model="showPopover" title="Select from possible opponents" width="200" :show-close="false" center style="border-radius: 4px;">
  <div style="margin: auto;">
    <ul style="list-style-type: none; padding: 0;">
      <li v-for="(opp, index) in opps" :key="index" style="padding: 8px 0;">
        <label style="cursor: pointer;">
          <input type="radio" v-model="selectedOpponent" :value="opp" @change="selectOpponent">
          <span style="color: black; font-weight: bold;">{{ opp.name }}</span>
        </label>
      </li>
    </ul>
  </div>
</el-dialog>
</template>

<script>
import { ref } from 'vue';
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";
import { AgGridVue } from "ag-grid-vue3";
import ButtonComponentVue from './components/ButtonComponent.vue';
import "element-plus";
import 'element-plus/dist/index.css'
import { ElDialog } from "element-plus"
import StateComponentVue from './components/StateComponent.vue'
import DateComponentVue from  './components/DateComponent.vue'
import ChallengerComponentVue from './components/ChallengerComponent.vue'

export default {
  name: "App",
  components: {
    AgGridVue,
    ElDialog,
  },
  setup() {
    const rowData = ref([]);
    const colDefs = ref([
      { field: "Pos", headerName: "Pos", width: "52", cellStyle: { 'font-size': '12px' } },
      { field: "name", headerName: "Player", width: "77", cellStyle: { 'font-size': '12px' } },
      { field: "hcp", headerName: "HCP", width: "56", cellStyle: { 'font-size': '12px' } },
      {
        headerName: "State", field: "state", width: "60", cellRenderer: StateComponentVue,
        cellRendererParams : (params) => ({
          player: params.data
        }),
        cellStyle : {'font-size':'12px'}
      },
      {
        headerName: "Date", field: "date", width:"69", cellStyle: { 'font-size': '12px' },
        cellRenderer: DateComponentVue,
        cellRendererParams : (params) => ({
          player: params.data
        })
      },
      {
        field: "challengers",
        headerName: "Possible Challengers", width:"250",        
        cellRenderer: ChallengerComponentVue, cellStyle: { 'font-size': '12px' },
        cellRendererParams: (params) => ({
          player: params.data,
          challenger: params.value
        })
      },
      {
        headerName: "",
        field: "action",
        width: "38",
        cellRenderer: ButtonComponentVue,
        cellRendererParams: (params) => ({ showChallengers: () => showChallengers(params.rowIndex),
          player: params.data
        }), cellStyle: { 'font-size': '12px' } 
      },
    ]);

    const rowheight = ref(30);
    const opps = ref([]);
    const showPopover = ref(false),
    selectedOpponent = ref(null)
    const challenger = ref(null)
    async function fetchPlayerData() {
      try {
        const response = await fetch('http://127.0.0.1:5000/api/players');
        const data = await response.json();
        const ParsedData = data.map((player, index) => ({
          Pos: index + 1,
          name: player.name,
          hcp: player.hcp,
          challengers: player.challengers,
          inGame: player.inGame
        }));
        rowData.value = ParsedData;
      } catch (err) {
        console.error("error fetching data: ", err);
      }
    }
    
    function showChallengers(rowIndex) {
      opps.value = rowData.value[rowIndex].challengers;
      challenger.value = rowData.value[rowIndex].name
      showPopover.value = true;
    }

    async function selectOpponent () {
      if(this.selectedOpponent) {
        try{
          const response = await fetch(`http://127.0.0.1:5000/api/challenge/${challenger.value}`,{
            method:'POST',
            headers :{
              'Content-type' : 'application/json'
            },
            body: JSON.stringify({challenged: this.selectedOpponent.name})
          })
          if (response.ok) {
            console.log("Challenge set Successfully");
            await fetchPlayerData();
            window.location.reload()
          } else {
            console.error("Failed to set Challenged", response.statusText)
          }

        }catch(err){
          console.error("Error:", err)
        }
        this.showPopover = false;
      }
    }

     fetchPlayerData(); 

    return {
      rowData,
      colDefs,
      rowheight,
      opps,
      showChallengers,
      showPopover,
      selectedOpponent,
      selectOpponent
    };
  },
};
</script>

<style>
.ag-theme-quartz {
  font-weight: normal;
  font-size: 12px;
}
</style>