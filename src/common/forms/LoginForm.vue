<template>
    <v-layout row justify-center>

        <v-btn v-show="!isLoggedIn" color="primary" @click="dialog=true" dark>Sign in</v-btn>
        <v-btn v-show="isLoggedIn" color="error" @click="logout" dark>Sign out ({{user}})</v-btn>

        <v-dialog v-model="dialog" max-width="420">
            <form class="login" @submit="login">
                <v-card>
                    <v-card-title class="headline">Sign in</v-card-title>
                    <v-card-text>User/1255, Enginer/1244, Supervisor/1233</v-card-text>
                    <v-card-text>
                        <div class="selectLogin">
                            <v-btn-toggle mandatory v-model="user" class="login-btn-group mb-4">
                                <v-btn flat value="User">User</v-btn>
                                <v-btn flat value="Engineer">Enginer</v-btn>
                                <v-btn flat value="Supervisor">Supervisor</v-btn>
                            </v-btn-toggle>
                        </div>

                        <div class="buttons">
                            <div class="button" @click="clickedButton(1)">1</div>
                            <div class="button" @click="clickedButton(2)">2</div>
                            <div class="button" @click="clickedButton(3)">3</div>
                            <div class="button" @click="clickedButton(4)">4</div>
                            <div class="button" @click="clickedButton(5)">5</div>
                            <div class="button" @click="clickedButton(6)">6</div>
                            <div class="button" @click="clickedButton(7)">7</div>
                            <div class="button" @click="clickedButton(8)">8</div>
                            <div class="button" @click="clickedButton(9)">9</div>
                        </div>

                        <input class="passwordInput" required v-model="password" type="password" placeholder=""/>

                    </v-card-text>

                    <v-card-actions>
                        <v-btn v-show="!isLoggedIn" type="submit" color="green darken-1" flat="flat">Login</v-btn>
                        <v-btn v-show="isLoggedIn" color="red darken-1" flat="flat" @click="logout">Logout</v-btn>
                    </v-card-actions>
                </v-card>
            </form>
        </v-dialog>
        <v-snackbar v-model="showMessage"
                    :timeout="2000"
                    :top="true"
                    :color=snackbarColor
        >
            {{ message }}
        </v-snackbar>
    </v-layout>
</template>

<script>
    import Vue from "vue";
    import Axios from "axios";
    import VueAxios from "vue-axios";

    Vue.use(VueAxios, Axios);

    export default Vue.extend({
        name: 'loginForm',
        data: () => ({
            amount: null,
            user: '',
            username: '',
            password: '',
            message: '',
            snackbarColor: 'error',
            showMessage: false,
            dialog: false
        }),
        mounted: function () {
            this.user = this.$store.getters.getUserRole;
        },
        computed: {
            getUserRole () {
                return this.$store.getters.getUserRole;
            },
            isLoggedIn () {
                return this.$store.getters.isLoggedIn;
            }
        },
        methods: {
            showSnackBar: function(message, color) {
                this.snackbarColor = color;
                this.message = message;
                this.showMessage = true;
            },
            clickedButton: function (num) {
                this.password += num;
                if (this.password.length === 4) {
                    this.login(this.user, this.password);
                    this.password = '';
                }
            },
            login: function () {
                let users = {"User": "1255", "Engineer": "1244", "Supervisor": "1233"};
                if (users[this.user] === this.password) {
                    this.dialog = false;
                    this.showSnackBar("Welcome, "+this.user+"!", "success");
                    this.$store.dispatch("login", this.user).then(() => {
                        this.$router.push("/");
                    }).catch(() => {});
                    return true;
                }
                else {
                    this.showSnackBar("Wrong Password! Try Again!", "error");
                    return false;
                }

            },
            logout() {
                let user = this.getUserRole;
                this.$store.dispatch("logout").then(() => {
                    this.$router.push("/");
                    this.showSnackBar("Goodbye, "+user+"!", "info");
                }).catch();
            }
        }
    })
</script>

<style>
    .buttons {
        display: flex;
        font-family: Helvetica, Arial, sans-serif;
        font-weight: 400;
        flex-wrap: wrap;
        justify-content: center;
        margin: 0 auto;
        width: 100%;
    }

    .button {
        border: 1px solid #006494;
        border-radius: 50px;
        color: #006494;
        cursor: pointer;
        display: inline-block;
        font-size: 26px;
        height: 80px;
        line-height: 80px;
        margin: 10px;
        text-align: center;
        width: 80px;
        -webkit-transition: all 0.3s;
        transition: all 0.3s;
    }

    .button:hover {
        background-color: #006494;
        color: #FFFFFF;
    }

    .passwordInput {
        font-size: 30px;
        text-align: center;
        display: block;
        margin: 20px auto 0;
    }

    .selectLogin {
        text-align:center;
    }

    @keyframes fade {
        0%, 100% {
            opacity: 0
        }
        50% {
            opacity: 1
        }
    }
</style>