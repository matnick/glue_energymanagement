<template>
   <v-container grid-list-md fill-height fluid>
            <v-layout class="fix-layout" row wrap>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(power_quality.access)">
                  <v-card :color="power_quality.color" to="/power_quality">
                     <div class="chart-background">
                        <bar-chart :data="power_quality.chartData" :hideAxis="true" :isDashboard="true"></bar-chart>
                     </div>
                     <v-card-title primary class="title">Power quality index
                        <v-spacer></v-spacer>
                        <v-icon>fa-weight</v-icon>
                     </v-card-title>
                     <v-card-text>
                        <div class="pb-3 display-2">
                           <span>PQI: 2</span> <br>
                        </div>
                        <div class="title">
                           <span>Power factor index: 1.2</span> <br>
                           <span>Total harmonic distortion index: 1.7</span> <br>
                           <span>Frequency deviation index: 1.2</span> <br>
                           <span>Voltage deviation index: 1</span> <br>
                           <span>Phase imbalance index: 2</span> <br>
                        </div>
                     </v-card-text>
                  </v-card>
               </v-flex>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(phase_current_balance.access)">
                  <v-card class="phase-card" :color="phase_current_balance.color" to="/phase_current_balance">
                     <div class="voltage-background">
                        <voltage-chart :value="phase_current_balance.line_l1" line="L1"></voltage-chart>
                        <voltage-chart :value="phase_current_balance.line_l2" line="L2"></voltage-chart>
                        <voltage-chart :value="phase_current_balance.line_l3" line="L3"></voltage-chart>
                     </div>
                     <v-card-title primary class="title">Phase current balance
                        <v-spacer></v-spacer>
                        <v-icon>fa-balance-scale</v-icon>
                     </v-card-title>
                  </v-card>
               </v-flex>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(scheduled_maintenance.access)">
                  <v-card :color="scheduled_maintenance.color" to="/scheduled_maintenance">
                     <v-card-title primary class="title">Scheduled maintenance
                        <v-spacer></v-spacer>
                        <v-icon>fa-clock</v-icon>
                     </v-card-title>
                     <v-card-text>
                        <div class="pb-3 display-1">
                           <span>Maintenance is not required</span> <br>
                        </div>
                        <div class="pt-4 headline">
                           <span>Next Service:</span> <br>
                        </div>
                        <div class="title">
                           <span>Compressor (AR-2): in 578 hours</span> <br>
                           <span>Refrigeration unit (FE-4): in 2879 hours</span> <br>
                        </div>
                     </v-card-text>
                  </v-card>
               </v-flex>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(electrical_shields.access)">
                  <v-card :color="electrical_shields.color" to="/electrical_shields">
                     <div class="chart-background">
                        <bar-chart :data="electrical_shields.chartData" :hideAxis="true" :isDashboard="true"></bar-chart>
                     </div>
                     <v-card-title primary class="title">Electrical shields status
                        <v-spacer></v-spacer>
                        <v-icon>fa-bolt</v-icon>
                     </v-card-title>
                     <v-card-text>
                        <div class="headline">
                           <span>Shields temperature:<br> within normal limits</span> <br> <br>
                           <span>Shields current:<br> within normal limits</span> <br>
                        </div>
                     </v-card-text>
                  </v-card>
               </v-flex>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(problems.access)">
                  <v-card :color="problems.color" to="/problems">
                     <v-card-title primary class="title">Problems and failures
                        <v-spacer></v-spacer>
                        <v-icon>fa-exclamation-triangle</v-icon>
                     </v-card-title>
                     <v-card-text>
                        <div class="pb-2 display-1">
                           <span>Сompressor (AR-2): <br>maximum current limit exceeded</span> <br>
                        </div>
                        <div class="headline pt-5">
                           <span>Days without Incidents: 0</span> <br>
                        </div>
                     </v-card-text>
                  </v-card>
               </v-flex>
               <v-flex class="flex-inner" d-flex md4 v-if="check_access(energy_consumption.access)">
                  <v-card :color="energy_consumption.color" to="/energy_consumption">
                     <div class="chart-background">
                        <bar-chart :data="energy_consumption.chartData" :hideAxis="true" :isDashboard="true"></bar-chart>
                     </div>
                     <v-card-title primary class="title">Energy consumption profiles
                        <v-spacer></v-spacer>
                        <v-icon>fa-lightbulb</v-icon>
                     </v-card-title>
                     <v-card-text>
                        <div class="pb-2 display-1">
                           <span>No deviations from the profiles</span> <br>
                        </div>
                     </v-card-text>
                  </v-card>
               </v-flex>
            </v-layout>
   </v-container>
</template>

<script>
import Vue from "vue";
import Axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, Axios);

import BarChart from "./common/charts/BarChart";
import VoltageChart from "./common/charts/VoltageChart.vue";

export default {
  components: {
    BarChart,
    VoltageChart
  },
  data: () => ({
    power_quality: {
      color: "nokia_red",
      access: "User|Engineer|Supervisor",
      chartData: [
        { title: "5 июля", value: 1 },
        { title: "6 июля", value: 2 },
        { title: "7 июля", value: 1 },
        { title: "8 июля", value: 1 },
        { title: "9 июля", value: 1 },
        { title: "10 июля", value: 1 },
        { title: "11 июля", value: 2 }
      ]
    },
    phase_current_balance: {
      access: "User|Engineer|Supervisor",
      color: "nokia_green",
      line_l1: 240,
      line_l2: 236,
      line_l3: 238
    },
    scheduled_maintenance: {
      access: "Engineer|Supervisor",
      color: "nokia_green"
    },
    electrical_shields: {
      access: "Engineer|Supervisor",
      color: "nokia_green",
      chartData: [
        { title: "5 июля", value: 1 },
        { title: "6 июля", value: 1 },
        { title: "7 июля", value: 1 },
        { title: "8 июля", value: 1 },
        { title: "9 июля", value: 2 },
        { title: "10 июля", value: 3 },
        { title: "11 июля", value: 4 }
      ]
    },
    problems: {
      access: "Supervisor",
      color: "nokia_yellow"
    },
    energy_consumption: {
      access: "User|Engineer|Supervisor",
      color: "nokia_green",
      chartData: [
        { title: "5 июля", value: 1 },
        { title: "6 июля", value: 2 },
        { title: "7 июля", value: 1 },
        { title: "8 июля", value: 1 },
        { title: "9 июля", value: 2 },
        { title: "10 июля", value: 4 },
        { title: "11 июля", value: 3 }
      ]
    }
  }),
    methods: {
      check_access (access_field) {
          if (access_field.indexOf(this.getUserRole)!==-1){
              return true
          }
          else {
              return false;
          }
      }
    },
    computed: {
        getUserRole () {
            return this.$store.getters.getUserRole;
        },
        isLoggedIn () {
            return this.$store.getters.isLoggedIn;
        }
    }
};
</script>

<style>

 .container.fill-height .layout.fix-layout {
  height: calc(100% + 8px);
  min-height:300px;
}

.container.fill-height .layout.fix-layout-large {
  height: calc(100% + 16px);
  min-height:300px;
}

 .container.fill-height {
    align-items: flex-start;
 }

 .flex-inner {
    max-height:50%;
 }


 .move-top {
  margin-top: -8px;
}

.chart {
  height: calc(100% - 56px);
}

.card {
  z-index: 2;
  overflow: hidden;
}

.chart-background {
  position: absolute;
  top: 0px;
  left: 0px;
  height: 100%;
  width: 100%;
  opacity: 0.3;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  z-index: 1;
}

.voltage-background {
  position: absolute;
  bottom: 0px;
  left: 0px;
  height: 80%;
  width: 100%;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  z-index: 1;
}

.voltage-background > * {
  margin-right: 32px;
}

.voltage-background > *:last-of-type {
  margin-right: 0px;
}

@media (max-width: 734px) {
  .phase-card {
    min-height: 300px;
  }
}

@media (max-width: 1200px) {
  .voltage-background > * {
    margin-right: 8px;
  }
}
</style>
