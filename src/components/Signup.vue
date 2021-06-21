<template>
  <div class="maincontainer_form">
<div class="signin_form_div">
  <h1>{{this.response_signin}}</h1>
  <h1 class="signup_header">Sign Up</h1>
  <h2 class="response_signup">{{ this.response_signup }}</h2>
<input type="email"
:class="{ form_signup: formclassbinding }"
v-model="email"
class="input_signup"
placeholder="Enter Youre Email:"
required
/>
<br /> 
<input type="text"
v-model="name"
class="input_signup"
placeholder="Enter Youre FullName:"
:class="{ form_signup: formclassbinding }"
required
/>
<br />
<input type="password"
v-model="password"
class="input_signup"
placeholder="Enter Youre Password"
:class="{ form_signup: formclassbinding }"
required
/>
<br />
  <vue-recaptcha 
  :sitekey="this.google_recaptcha_sitekey"
   :loadRecaptchaScript="true"
   @verify="recaptchaVerified"
:class="{ form_signup: formclassbinding }"
   ></vue-recaptcha>
  <br />
<button
type="submit"
@click="send_signup"
:class="{ form_signup: formclassbinding }"
class="btn_signup"

>Sign Up</button>
<h2 
v-for="resultforms in resultform" :key="resultforms.message"
style="background-color:red;color:white;">{{resultforms }}</h2>
<router-link to="/Signin"
style="color:white"
>Sign In</router-link>
</div>
</div>
</template>

<script>
import VueRecaptcha from 'vue-recaptcha'
import axios from 'axios'
import Configs from '../assets/js/Configs'
export default {
    components: { VueRecaptcha },
  data(){
return{
email:"",
name:"",
password:"",
google_recaptcha_sitekey: Configs.google_recaptcha_sitekey,
recaptcha_token:"",
resultform:{
  result_else_email:"",
result_else_nameandfamily:"",
result_else_password:"",
result_else_recaptcha:"",
},
response_signin:[],
response_signup:"",
formclassbinding: false
}
  },
  methods:{
    recaptchaVerified(recaptchaToken){
  this.recaptcha_token = recaptchaToken
},
send_signup(){
  if(this.recaptcha_token && this.email && this.name && this.password && this.password.length > 3 && this.name.length > 2){
axios
.post(Configs.api_server_address + '/users/signup',{
   email: this.email,
   password: this.password,
   full_name: this.name,
   recaptcha: this.recaptcha_token,
})
.then(response=>this.response_signin = response)
.then(response=>console.log(response))
.catch(err=>'in error ro dari:'+ err)
  }
 if(!this.email){
    this.resultform.result_else_email="لطفا ایمیل را پر کنید"
}
 if(this.email){
    this.resultform.result_else_email=""
}
 if(!this.name){
  this.resultform.result_else_nameandfamily="لطفا نام و نام خانوادگی را پر کنید"
}
 if(this.name){
  this.resultform.result_else_nameandfamily=""
}
 if(!this.password){
    this.resultform.result_else_password="لطفا پسورد را پر کنید"
}
 if(this.password){
    this.resultform.result_else_password=""
}


 if(!this.recaptcha_token){
  this.resultform.result_else_recaptcha="لطفا ریکپچا را تایید کنید"
}
 if(this.recaptcha_token){
  this.resultform.result_else_recaptcha=""
}
if(this.response_signin.data.code == 200){
  this.formclassbinding = true,
  this.response_signup= "ثبت نام با موفقیت انجام شد"
}
else if(!this.response_signin.data.code == 200){
  this.response_signup= "",
  this.response_signup= "ثبت نام انجام نشد"
}
window.grecaptcha.reset()
},
  }
}
</script>
<style scoped>
.response_signup{
  background-color: green;
  text-align: center;
  color:white
}
.form_signup{
  display:none
}
.signin_form_div{
background-color:rgb(34, 54, 44);
width:700px;
height:700px;
  display:flex;
  flex-direction: column;
justify-content: center;
align-items: center;
}
.maincontainer_form{
  display:flex;
justify-content: center;
align-items: center;
height:900px;
background-color:rgb(40, 66, 22);
}
.btn_signup{
  width:90px;
  height:50px;
  background: green;
  color:white;
  border-radius: 10%;
}
.input_signup{
  height:50px;
  width:400px;
  text-align:center;
}
.signup_header{
  color:white;
  background: rgb(73, 1, 1);
  width:250px;
height:60px;
text-align: center;
padding-top:20px;
}
</style>