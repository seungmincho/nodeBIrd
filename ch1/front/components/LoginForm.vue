<template>
    <v-container v-if="!me">
        <v-card>
            <v-form ref="form" v-model="valid" @submit.prevent="onLoginForm">
                <v-container>
                    <v-text-field
                        v-model="email"
                        label="이메일"
                        type="email"
                        :rules="emailRules"
                        required
                        prepend-icon="mdi-account"
                    />
                    <v-text-field
                        v-model="password"
                        label="비밀번호"
                        type="password"
                        :rules="passwordRules"
                        required
                        prepend-icon="mdi-lock"
                    />
                    <v-btn color="green" type="submit" :disabled="!valid">로그인</v-btn>
                    <v-btn nuxt to="/signup">회원가입</v-btn>
                </v-container>
            </v-form>
        </v-card>
    </v-container>
    <v-container v-else>
        <v-card>
            <v-container>
                <div>안녕하세요 {{ me.nickname }}님</div>
                <v-btn @click="onLogout">로그아웃</v-btn>
            </v-container>   
        </v-card>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            valid: false,
            email: '',
            password: '',
            emailRules: [
                v => !!v || '이메일은 필수입니다.',
                v => /^[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]{2,3}$/i.test(v) || '유효하지 않은 이메일입니다.'
            ],
            passwordRules: [
                v => !!v || '비밀번호는 필수입니다.',
            ],
        }
    },
    computed: {
        me() {
            return this.$store.state.users.me;
        },
    },
    methods: {
        async onLoginForm(){
            if(this.$refs.form.validate()){
                try{
                    await this.$store.dispatch('users/logIn', {
                        email: this.eamil,
                        password: this.password,
                        nickname: '조승민',
                    })
                }catch{
                        alert('로그인 실패');

                }  
                this.$router.push({
                    path: '/'
                });       
            }else{
                alert('잘못입력');
            }
        },
        onLogout() {
            this.$store.dispatch('users/logOut')
        }
    },
}
</script>

<style lang="stylus">

</style>