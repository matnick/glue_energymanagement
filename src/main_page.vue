<template lang=html>
   <v-app id="inspire">
      <v-navigation-drawer
         :clipped="true"
         :mini-variant.sync="menu_collapsed"
         fixed app disable-resize-watcher permanent touchless width=250 mini-variant-width=60
      >
         <v-list dense>
            <template v-for="item in items">
               <v-layout v-if="item.heading" :key="item.heading" row align-center >
                  <v-flex xs6>
                     <v-subheader v-if="item.heading">
                        {{ item.heading }}
                     </v-subheader>
                  </v-flex>
                  <v-flex xs6 class="text-xs-center">
                     <a href="#!" class="body-2 black--text">EDIT</a>
                  </v-flex>
               </v-layout>
               <v-list-group
                  v-else-if="item.children"
                  :key="item.text"
                  :prepend-icon="item.model ? item.icon : item.alticon"
                  append-icon=""
                  v-bind:value="get_expanded_status(item.path)"
               >
                  <v-list-tile slot="activator">
                     <v-list-tile-content>
                        <v-list-tile-title>
                           {{ item.text }}
                        </v-list-tile-title>
                     </v-list-tile-content>
                  </v-list-tile>
                  <v-list-tile
                     v-for="(child, i) in item.children"
                     :key="i"
                     :to="{path: child.path}"
                  >
                     <v-list-tile-action v-if="child.icon">
                        <v-icon small class="ml-2">{{ child.icon }}</v-icon>
                     </v-list-tile-action>
                     <v-list-tile-content>
                        <v-list-tile-title class="ml-2">
                           {{ child.text }}
                        </v-list-tile-title>
                     </v-list-tile-content>
                  </v-list-tile>
               </v-list-group>
               <v-list-tile v-else v-show="check_access(item.access)" :key="item.text" :to="{path: item.path}">
                  <v-list-tile-action>
                     <v-icon>{{ item.icon }}</v-icon>
                  </v-list-tile-action>
                  <v-list-tile-content>
                     <v-list-tile-title>
                        {{ item.text }}
                     </v-list-tile-title>
                  </v-list-tile-content>
               </v-list-tile>
            </template>
         </v-list>
      </v-navigation-drawer>
      <v-toolbar color="primary" dark app fixed clipped-left class="toolbar" height=48>
         <v-btn icon class="burger" @click="toggle_menu"><v-icon>fa-bars</v-icon></v-btn>
         <v-menu>
            <v-toolbar-title  slot="activator">
               <img class="mr-3 logo" src="./assets/logo_nokia.svg" height="25px" alt="Nokia Logo">
               <p>Energy management</p>
            </v-toolbar-title>
         </v-menu>
         <v-spacer></v-spacer>
         <v-toolbar-items class="hidden-sm-and-down powered">
            <div class="title" >Powered by IMPACT</div>
         </v-toolbar-items>
      </v-toolbar>
      <v-content>
         <router-view></router-view>
      </v-content>
       <loginForm></loginForm>
   </v-app>
</template>

<script>
    import loginForm from './common/forms/LoginForm.vue'

    export default {
        components: {
            loginForm,
        },
        data() {
            return {
                menu_collapsed: true,
                items: [
                    {
                        path: "dashboard",
                        text: "Dashboard",
                        icon: "fa-tachometer-alt",
                        access: "User|Engineer|Supervisor|Guest"
                    },
                    {
                        path: "power_quality",
                        text: "Power quality index",
                        icon: "fa-weight",
                        access: "User|Engineer|Supervisor|Guest"
                    },
                    {
                        path: "phase_current_balance",
                        text: "Phase current balance",
                        icon: "fa-balance-scale",
                        access: "User|Engineer|Supervisor"
                    },
                    {
                        path: "scheduled_maintenance",
                        text: "Scheduled maintenance",
                        icon: "fa-clock",
                        access: "Engineer|Supervisor"
                    },
                    {
                        path: "electrical_shields",
                        text: "Electrical shields status",
                        icon: "fa-bolt",
                        access: "Engineer|Supervisor"
                    },
                    {
                        path: "problems",
                        text: "Problems and failures",
                        icon: "fa-exclamation-triangle",
                        access: "Supervisor"
                    },
                    {
                        path: "energy_consumption",
                        text: "Energy consumption profiles",
                        icon: "fa-lightbulb",
                        access: "User|Engineer|Supervisor"
                    }
                ]
            }
        },
        methods: {
            get_expanded_status(path) {
                if (window.location.href.includes(path)) {
                    return true;
                }

                return false;
            },
            check_access (access_field) {
                if (access_field.indexOf(this.getUserRole)!==-1){
                    return true
                }
                else {
                    return false;
                }
            },
            toggle_menu() {
                this.menu_collapsed = !this.menu_collapsed;
            },
            logout() {
                this.$store.dispatch("logout");
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
.toolbar {
  z-index: 10;
}

.v-navigation-drawer .v-list__tile {
  padding: 0 12px;
}

.v-navigation-drawer .v-list__tile__action {
  min-width: 36px;
}

.container {
  padding: 12px;
}

.v-toolbar__title {
   display: flex;
   align-items: center;
}

.v-toolbar__title p {
   margin: 0;
   margin-bottom: 2px;
}

.burger {
   margin-top: 0px !important;
   margin-bottom: 0px !important;
}

.powered {
   display: flex;
   align-items: center;
}

.powered .title {
   margin-bottom: 3px;
}

.logo {
   margin-top: 1px;
}
</style>
