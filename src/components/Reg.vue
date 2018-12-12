<template>
    <transition name="slide-fade">
    <div class="reg">
        <h2>註冊</h2>
        <div class="regWrap">
            <div>
                <span>帳號</span>
                <span><input type="text" v-model="accountC" @blur="accountCK" placeholder="*帳號(請輸入6~24個英文或數字半形字元)"></span>
                <span class="checkStyle">{{accountText}}</span>
            </div>
            <div>
                <span>密碼</span>
                <span><input type="password" v-model="passwordC" @blur="passwordCK" placeholder="*密碼(請輸入6~24個英文或數字半形字元)"></span>
                <span class="checkStyle">{{passwordText}}</span>
            </div>
            <div>
                <span>確認密碼</span>
                <span><input type="password" v-model="psdC" @blur="psdCK" placeholder="*再次輸入密碼"></span>
                <span class="checkStyle">{{checkPsd}}</span>
            </div>
            <div>
                <span>電子信箱</span>
                <span><input type="email" v-model="emailC" @blur="emailCK" placeholder="*example@gmail.com"></span>
                <span class="checkStyle">{{emailText}}</span>
            </div>
            <div>
                <span>姓名</span>
                <span><input type="text" v-model="nameC" @blur="nameCK" placeholder="*姓名"></span>
                <span class="checkStyle">{{nameText}}</span>
            </div>
            <div>
                <span>手機</span>
                <span><input type="tel" v-model="phoneC" @blur="phoneCK" placeholder="*手機號碼"></span>
                <span class="checkStyle">{{phoneText}}</span>
            </div>
            <div>
                <span>地址</span>
                <c-address
                    @getData = "getData"
                    @ctGetData = "ctGetData"
                ></c-address>
                <input type="text" class="address" placeholder="地址" v-model="addressText">
            </div>
            <!-- <div>
                <span>會員條款</span>
                <span><input type="checkbox">我已詳閱會員條款</span>
            </div> -->
            <button class="submitBtn" @click="sumbitData">送出資料</button>
        </div>
    </div>
    </transition>
</template>

<script>
import CAddress from './CAddress'
export default {
    data(){
        return{
            accountC:null,
            accountText:"",
            passwordC:null,
            passwordText:"",
            psdC:null,
            checkPsd:"",
            emailC:null,
            emailText:"",
            nameC:null,
            nameText:"",
            phoneC:null,
            phoneText:"",
            addressTitle:"200基隆市仁愛區",
            addressText:null
        }
    },
    components:{
        CAddress
    },
    methods:{
        accountCK(){
            
            if(this.accountC === null || this.accountC === ""){
                this.accountText = "帳號不得為空白"
            }else{
                if(/^.[A-Za-z0-9]+$/.test(this.accountC) === true){
                    this.accountText = ""
                }else{
                    this.accountText = "帳號不符合規定(請輸入6~24個英文或數字半形字元)"
                }
            }
        },
        passwordCK(){
            if(this.passwordC === null || this.passwordC === ""){
                this.passwordText = "密碼不得為空白"
            }else{
                if(/^.[A-Za-z0-9]+$/.test(this.passwordC) === true){
                    this.passwordText = ""
                }else{
                    this.passwordText = "密碼不符合規定(請輸入6~24個英文或數字半形字元)"
                }
            }
        },
        psdCK(){
            this.psdC !== this.passwordC ? this.checkPsd = "與密碼不符": this.checkPsd = ""
        },
        emailCK(){
            let emailRule = /^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z]+$/
            if(this.emailC === null || this.emailC === ""){
                this.emailText = "電子信箱不得為空白"
            }else{
                if(emailRule.test(this.emailC) === true){
                    this.emailText = ""
                }else{
                    this.emailText = "Email不符合格式"
                }
            }
        },
        nameCK(){
            this.nameC === null || this.nameC === "" ?this.nameText = "名字不得為空白":this.nameText = ""
        },
        phoneCK(){
            console.log(this.phoneC)
            this.phoneC === null || this.phoneC === "" ?this.phoneText = "電話不得為空白":this.phoneText = ""
        },
        getData(value){
            let address = value[2]+value[0]+value[1]
            this.addressTitle = address
            console.log(value)
        },
        ctGetData(value){
            let address1 = value[2]+value[0]+value[1]
            this.addressTitle = address1
            console.log(value)
        },
        sumbitData(){
            if(this.accountC === null || this.accountText === "帳號不符合規定(請輸入6~24個英文或數字半形字元)"){
                alert('請檢查帳號')
            }else if(this.passwordC ===null || this.passwordText === "密碼不符合規定(請輸入6~24個英文或數字半形字元)"){
                alert('請檢查密碼')
            }else if(this.psdC === null || this.checkPsd === "與密碼不符"){
                alert('請檢查確認密碼')
            }else if(this.emailC === null || this.emailText === "Email不符合格式"){
                alert('請檢查電子信箱')
            }else if(this.nameC === null || this.nameText  === "名字不得為空白"){
                alert('請檢查姓名')
            }else if(this.phoneC ===null || this.phoneText === "電話不得為空白"){
                alert('請檢查手機號碼')
            }else{
                let ckData = JSON.parse(localStorage.getItem('user'))
                console.log(ckData)
                if(ckData){
                    ckData.push({
                        account:this.accountC,
                        password:this.passwordC,
                        email:this.emailC,
                        name:this.nameC,
                        phone:this.phoneC,
                        address:this.addressTitle+this.addressText
                    })
                    localStorage.setItem('user',JSON.stringify(ckData))
                }else{
                    let userData = []
                    userData.push({
                        account:this.accountC,
                        password:this.passwordC,
                        email:this.emailC,
                        name:this.nameC,
                        phone:this.phoneC,
                        address:this.addressTitle+this.addressText
                    })
                    localStorage.setItem('user',JSON.stringify(userData))
                }
                
                // localStorage.setItem('newAccount',JSON.stringify(this.accountC))
                // localStorage.setItem('newPassword',JSON.stringify(this.passwordC))
                // localStorage.setItem('newEmail',JSON.stringify(this.emailC))
                // localStorage.setItem('newName',JSON.stringify(this.nameC))
                // localStorage.setItem('newPhone',JSON.stringify(this.phoneC))
                // localStorage.setItem('newAddress',JSON.stringify(this.addressTitle+this.addressText))
                alert('註冊成功')
                this.$router.push({path:'/'})
            }
        }
    }
}
</script>

<style lang="scss">
.reg{
    width:500px;
    border:1px solid rgba(37, 6, 6, 0.3);
    margin:auto;
    box-sizing: border-box;
    background: rgba(250, 250, 250, 0.92);
    border-radius: 5px;
    box-shadow: 0px 4px 3px rgba(106, 131, 134, 0.1);
    .regWrap{
        padding:10px;
        box-sizing: border-box;
        text-align: left;
        input{
            margin-left: 5px;
            width:280px;
            height:24px;
        }
        select{
            height:30px;
        }
        div{
            padding-bottom: 10px;
        }
        div span:first-child{
            width:70px;
            display:inline-block;
            background-color: #DDD;
            height:30px;
            text-align: center;
            line-height: 30px;
        }
        .address{
            margin-top:10px;
            margin-left:75px;
        }
        input[type=checkbox]{
            width:15px;
            vertical-align: middle;
        }
    }
    h2{
        font-size:20px;
        font-family: $fontF;
        padding:20px 0px;
        text-align: center;
    }
    .submitBtn{
        display: block;
        background-color: #285f8f; 
        border: none;
        color: white;
        padding: 5px 32px;
        text-align: center;
        text-decoration: none;
        font-size: 16px;
        outline: none;
        border-radius: 4px;
        cursor: pointer;
        margin:20px auto;
        &:hover{
            background-color: #5aabf1;
            color:rgba(0,0,0,.7);
        }
    }
    .checkStyle{
        color:#F00;
        font-size:12px;
        font-family: $fontF;
    }
}
.slide-fade-enter-active {
  transition: all .5s cubic-bezier(.36,.99,1,1.49);
}
.slide-fade-leave-active {
  transition: all 0s;
}
.slide-fade-leave{
  opacity: 0;
}

.slide-fade-enter
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: scale(0.8);
  opacity: 0;
}
</style>

