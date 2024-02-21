<template>
    <div class="reg_div">
        <div class="head_reg">
            <div>
                <h1>
                    {{ `registration`.toUpperCase() }}
                </h1>
            </div>
            <div>
                <p>
                    Please enter your details
                </p>                
            </div>
        </div>
        <div class="mid_reg">
            <div class="input_div">
                <div>
                    <p>Login:</p>
                </div>
                <div>
                    <input v-model="regData.login" type="text" placeholder="Login" class="login">
                </div>                
            </div>
            <div class="input_div">
                <div>
                    <p>Password:</p>
                </div>
                <div>
                    <input v-model="regData.password" type="text" placeholder="Password" class="password">
                </div>                
            </div>
            <div class="input_div">
                <div>
                    <p>Try Password:</p>
                </div>
                <div>
                    <input v-model="regData.tryPassword" type="text" placeholder="try password" class="tryPassword">
                </div>                
            </div>
            <div class="forgot_div">
                <div></div>
                <div>
                    <p class="forgot_password">Forgot Password?</p>
                </div>                
            </div>
        </div>
        <div class="foot_reg">
            <div class="sign_up_btn" v-on:click="registrationClickHandler">
                <p>
                    Sign up
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
            <div class="already_have_acc_btn" @click="switch_auth_reg_handler">
                <p>
                    Already have an account
                </p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Reg',        
        data: () => ({
            serverUrl: 'http://195.49.210.34/',
            regData: {
                login: '',
                password: '',
                tryPassword: ''
            },
            // response: ''
        }), 
        methods: {
            switch_auth_reg_handler: function () {
                this.$emit('switch_auth_reg_handler');
            },
            registrationClickHandler: async function () {
                if(!this.regData.login || !this.regData.password || !this.regData.tryPassword) return alert('заполните поля')
                const request_body = {
                    userData: {
                        login: this.regData.login,
                        password: this.regData.password,
                        tryPassword: this.regData.tryPassword
                    }
                }
                const response = await this.sendRequest('user/registration', 'POST', request_body)
                console.log({response});
                if(response.info.status == "Error") return alert(response.payload)

                else if(response.info.status == "OK")
                this.$router.push({name: 'home', params: {id: response.payload._id}})

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

<style lang="less" scoped>
    * {
        margin: 0;
        padding: 0;
    }
    .reg_div {
        font-family: Arial, Helvetica, sans-serif;
        padding: 30px;
        border-radius: 30px;
        border: blue;
        background-color: white;
        width: 400px;
        display: grid;
        grid-gap: 20px;
        // box-shadow: 0 0 3px;
        .head_reg {
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
        .mid_reg {
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
        .foot_reg {
            display: grid;
            grid-gap: 10px;
            text-align: center;
            .sign_up_btn {
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
            .already_have_acc_btn {
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