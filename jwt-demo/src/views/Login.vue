<template>
    <v-app id="inspire">
        <v-content>
            <v-container fluid fill-height>
                <v-layout align-center justify-center>
                    <v-flex xs12 sm8 md4>
                        <v-card class="elevation-12">
                            <v-toolbar dark color="primary">
                                <v-toolbar-title>登录</v-toolbar-title>
                            </v-toolbar>
                            <v-card-text>
                                <v-form>
                                    <v-text-field prepend-icon="mdi-account" v-model="username" label="Username" type="text"></v-text-field>
                                    <v-text-field prepend-icon="mdi-lock" v-model="password" label="Password" type="password"></v-text-field>
                                </v-form>
                            </v-card-text>
                            <v-card-actions>
                                还没账号？前往<router-link to="/reg">注册</router-link>
                                <v-spacer></v-spacer>
                                <v-btn color="primary" @click="login">登录</v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
    export default {
        data: () => ({
            username: '',
            password: ''
        }),
        methods: {
            login() {
                if(this.username === '' || this.password === ''){
                    return this.$toast('请输入正确的用户名和密码');
                }
                this.$axios.post('/api/login', {
                    name: this.username,
                    password: this.password
                }).then(res => {
                    if(res.code === '000001'){
                        localStorage.setItem('token', res.data);
                        localStorage.setItem('token_exp', new Date().getTime());
                        this.$router.push('/');
                    }else{
                        this.$toast(res.msg);
                    }
                })
            }
        }
    }
</script>