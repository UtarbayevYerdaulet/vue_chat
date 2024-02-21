<template>
    <div class="login_page">
        <Auth v-show="switcher" @switch_auth_reg_handler="switch_handler"/>
        <Reg v-show="!switcher" @switch_auth_reg_handler="switch_handler"/>
        <!-- <h1>Welcome to My first Vue.js App</h1> -->
    </div>
</template>

<script>
    import Auth from '@/components/Auth.vue'
    import Reg from '@/components/Reg.vue'

    export default {
        name: 'Login',
        components: {
            Auth,
            Reg
        },
        data: () => ({
            switcher: true
        }),
        methods: {
            switch_handler: function () {
                this.switcher = !this.switcher
            }
        }
    }
</script>

<style lang="less" scoped>
.login_page {
    background-color: #e6b4a3;
    height: 100vh;
    display: grid;
    justify-content: center;
    align-content: center;
}
// .home {
//     // width: 100vw;
//     // height: 100vh;
//     // display: flex;
//     // flex-direction: column;
//     // justify-content: center;
//     // align-items: center;

//     // gap: 15px;
//     // .switcher {
//     //     width: 50%;
//     //     height: 45px;
//     //     padding: 8px 16px 8px 16px;
//     //     & * {
//     //         width: 25%;
//     //         cursor: pointer;
//     //         font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
//     //         font-size: 1.25em;
//     //     }
//     // }
// }
</style>
