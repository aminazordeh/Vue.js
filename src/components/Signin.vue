<template>
<div class="main_container_login">
    <h1 class="">This is Sign in</h1>
    <br />
    <div class="form_login">
<input type="email"
placeholder="pelese Enter Youre UserName:"
v-model="email" 
/>
<br />
<input type="password"
placeholder="pelese Enter Youre Password:"
v-model="password" 
/>
<br />
<button
type="submit"
@click="sendloginform()"
class=""
>Sign In</button>
    </div>






</div>
</template>
<script>
import VueRecaptcha from 'vue-recaptcha'
import axios from 'axios'
import Configs from '../assets/js/Configs'
export default {
    
data(){
return{
email:"",
password:"",
recaptcha:Configs.google_recaptcha_sitekey,
recaptchasend:"",
}
},
methods:{
sendloginform(){
axios
.post(Configs.api_server_address + '/users/signin',{
    email: this.email,
    password: this.password,
    recaptcha: this.recaptchasend,
})
.then(res=>console.log(res))

}


},


}
</script>

<style scoped>
.main_container_login{
    display:flex;
    flex-direction: column;
justify-content: center;
align-items: center;
}
    .form_login{
            display:flex;
    flex-direction: column;
justify-content: center;
align-items: center;
    }
</style>