<template>
  <transition name="slide-fade">
  <div class="login">
    <div class="loginBlock">
      <h1>{{ msg }}</h1>
      <span class="loginTitle">帳號</span>
      <input type="text" placeholder="帳號" v-model="account">
      <div class="loginPsd">密碼</div>
      <!-- <div class="forgetPwd">忘記密碼?</div> -->
      <input type="password" placeholder="密碼" v-model="password">
      <button class="loginBtn" @click="signIn">登入</button>
      <div class="divide"></div>
      <router-link to="/Reg"><button class="signBtn">註冊</button></router-link>
    </div>
  </div>
  </transition>
</template>

<script>
import Reg from './Reg'
export default {
  data () {
    return {
      msg: '會員登入',
      account: '',
      password:''
    }
  },
  components:{
    Reg
  },
  methods:{
    signIn(){
      let userInfo = JSON.parse(localStorage.getItem('user'))
      console.log(userInfo);
      for(var i=0;i<userInfo.length;i++){
        if(userInfo[i].account === this.account && userInfo[i].password === this.password){
          sessionStorage.setItem('userId',JSON.stringify(i))
          this.$router.push({path:'/Signin'})
          break
        }
      }
      // 如果迴圈跑完還是沒找到就代表沒這個帳密
      if(i===userInfo.length){
        alert('帳號密碼錯誤')
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
h1, h2 {
  font-weight: normal;
  font-family: '微軟正黑體';
  font-size:24px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #FFF;
  text-decoration:none;
}
span{
  font-family: '微軟正黑體';
}
.clearfix{
  clear: both;
}
// .router-link-active{
//   border-bottom:none;
// }
.login{
  width:300px;
  margin:auto;
  border:1px solid rgba(37, 6, 6, 0.3);
  min-height: 380px;
  padding:10px;
  box-sizing: border-box;
  background: rgba(250, 250, 250, 0.92);
  border-radius: 5px;
  box-shadow: 0px 4px 3px rgba(106, 131, 134, 0.1);
  h1{
    padding:20px 0px 30px 0px;
  }
  .loginBlock{
    text-align: left;
    .loginTitle{
      display: block;
      padding-bottom:5px;
    }
    .loginPsd{
      width:50%;
      float: left;
      padding-bottom:5px;
    }
    .forgetPwd{
      width:50%;
      text-align:right;
      font-size:14px;
      color:rgba(0, 0,0,.7);
      float:right;
      padding-bottom:5px;
    }
    h1{
      font-size:24px;
    }
    input{
      width:100%;
      padding:8px;
      margin-bottom:15px;
      box-sizing: border-box;
      font-size: .875rem;
      color: #222;
      outline: none;
      border:1px solid rgba(0,0,0,.14);
      box-shadow: inset 0 2px 0 0 rgba(0,0,0,.02);
      border-radius: 2px;
    }
    .loginBtn,.signBtn{
      display:block;
      width:100%;
      background-color: #285f8f; /* Green */
      border: none;
      color: white;
      padding: 5px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      outline: none;
      border-radius: 4px;
      cursor: pointer;
      margin-top:40px;
      &:hover{
        background-color: #5aabf1;
        color:rgba(0,0,0,.7);
      }
    }
    .signBtn{
      margin-top:10px;
      
    }
    .divide{
      width:100%;
      height:1px;
      padding-top:10px;
      border-bottom:1px solid rgba(127,127,127,.8);
    }
  }
}
.slide-fade-enter-active {
  // transition: all .5s ease;
  animation: slide-enter 1s;
}
.slide-fade-leave-active {
  transition: all 0s;
}
.slide-fade-leave{
  opacity: 0;
}
.slide-fade-enter
/* .slide-fade-leave-active for below version 2.1.8 */ {
  // transform: rotateY(360deg);
  opacity: 0;
}
@keyframes slide-enter{
  0% {
    transform: scale(0.5);
  }
  25% {
    transform: scale(1) rotate(45deg);
  }
  50%{
    transform: scale(1) rotate(-45deg);
  }
  100% {
    transform: scale(1);
  }
}
</style>
