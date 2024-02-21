<template>
    <div class="auth_div">
        <div class="head_auth">
            <div>
                <h1>
                    {{ `authorization`.toUpperCase() }}
                </h1>
            </div>
            <div>
                <p>
                    Please enter your details
                </p>                
            </div>
        </div>
        <div class="mid_auth">
            <div class="input_div">
                <div>
                    <p>Login:</p>
                </div>
                <div>
                    <input v-model="authData.login" type="text" placeholder="Login" class="login">
                </div>                
            </div>
            <div class="input_div">
                <div>
                    <p>Password:</p>
                </div>
                <div>
                    <input v-model="authData.password" type="text" placeholder="Password" class="password">
                </div>                
            </div>
            <div class="forgot_div">
                <div></div>
                <div>
                    <p class="forgot_password">Forgot Password?</p>
                </div>                
            </div>
        </div>
        <div class="foot_auth">
            <div class="sign_in_btn" v-on:click="buttonClickHandler">
                <p>
                    Sign in
                </p>
            </div>
            <div class="foot_wall">
                <div>
                    <hr>
                </div>
                <div>
                    <p>Or</p>
                </div>
                <div>
                    <hr>
                </div>
            </div>
            <div class="create_new_acc_btn" @click="switch_auth_reg_handler">
                <p>
                    Create new account
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
        name: 'Auth',        
        data: () => ({
            serverUrl: 'http://195.49.210.34/',
            authData: {
                login: '',
                password: ''
            },
            // response: ''
        }), 
    methods: {
        switch_auth_reg_handler: function () {
            this.$emit('switch_auth_reg_handler');
        },
        buttonClickHandler: async function () {
            if(!this.authData.login || !this.authData.password) return alert('заполните поля')
            const request_body = {
                userData: {
                    login: this.authData.login,
                    password: this.authData.password
                }
            }
            const response = await this.sendRequest('user/authorization', 'POST', request_body)

            if(response.info.status === 'OK' && response.payload.isAuth) {
                this.$router.push({name: 'home' })
                this.$router.push({name: 'home', params: {id: response.payload.userData[0]._id}})
            } else {
                alert('Логин или пароль некорректен')
            }


            console.log({response});
        },
        sendRequest: async function (path, method, body) {
            const request_config = {
                method,
                headers: {
                    'Content-Type': 'application/json;charset=utf-8'
                },
                body: null
            }

            if(method !== 'GET') request_config.body = JSON.stringify(body)
            console.log(JSON.stringify(body));

            // fetch(`${this.serverUrl}${path}`, request_config)
            // .then(response => {
            //     // console.log({response});
            //     requestData = response.json()
            //     .then(jsonRes => {
            //         this.response = jsonRes;
            //         // console.log(requestData);
            //     })
            //     .catch (error => {
            //         console.log({error});
            //     })
            // })
            // .catch (error => {
            //     console.log({error})
            // })
                let data = await fetch(`${this.serverUrl}${path}`, request_config)
                return await data.json()

            // console.log({response});
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
    * {
        margin: 0;
        padding: 0;
    }
    .auth_div {
        font-family: Arial, Helvetica, sans-serif;
        padding: 30px;
        border-radius: 30px;
        border: blue;
        background-color: white;
        width: 400px;
        display: grid;
        grid-gap: 20px;
        // box-shadow: 0 0 3px;
        .head_auth {
            border-left: 5px solid #f09273;
            display: grid;
            grid-gap: 2px;
            div {
                display: grid;
                padding-left: 10px;
                h1 {
                    text-align: start;
                }
                p {
                    text-align: start;
                    color: grey;
                }
            }
        }
        .mid_auth {
            display: grid;
            grid-gap: 10px;
            grid-template-rows: 1fr 1fr .1fr;
            .input_div {
                display: grid;
                grid-gap: 2px;
                div {
                    display: grid;
                    p {
                        text-align: start;
                        color: grey;
                    }
                    input {
                        outline: none;
                        border: none;
                        background-color: gainsboro;
                        font-size: 20px;
                        border-radius: 10px;
                        padding: 10px;
                        color: grey;
                    }
                }
            }
            .forgot_div {
                display: grid;
                grid-template-columns: 2fr 1fr;
                grid-gap: 10px;
                .forgot_password {
                    cursor: pointer;
                    text-align: end;
                    color: #f09273;
                }
            }
        }
        .foot_auth {
            display: grid;
            grid-gap: 10px;
            text-align: center;
            .sign_in_btn {
                color: white;
                background-color: #f09273;
                border-radius: 10px;
                cursor: pointer;
                p {
                    font-size: 26px;
                    padding: 10px;
                }
            }
            .foot_wall {
                display: grid;
                grid-template-columns: 5fr 1fr 5fr;
                grid-gap: 5px;
                div {
                    display: grid;
                    align-content: center;
                    p {
                        color: grey;
                    }
                }
            }
            .create_new_acc_btn {
                color: #f09273;
                border-radius: 10px;
                border: 3px solid #f09273;
                cursor: pointer;
                p {
                    font-size: 26px;
                    padding: 10px;
                }
            }
        }
    }
</style>
