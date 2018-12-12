<template>
    <div class="dataTable">
        <H1>帳戶資料</H1>
        <table>
            <tr>
                <th>姓名</th><td>{{ name }}</td>
            </tr>
            <tr>
                <th>帳號</th><td>{{ account }}</td>
            </tr>
            <tr>
                <th>密碼</th><td>{{ password }}</td>
            </tr>
            <tr>
                <th>手機</th><td>{{ phone }}</td>
            </tr>
            <tr>
                <th>電子郵件</th><td> {{ email}}</td>
            </tr>
            <tr>
                <th>地址</th><td>{{ address }}</td>
            </tr>
        </table>
    </div>
    
</template>

<script>
export default {
    data(){
        return{
            userId:null,
            name:null,
            account:null,
            password:null,
            phone:null,
            email:null,
            address:null
        }
    },
    created:function(){
        this.userId = JSON.parse(sessionStorage.getItem('userId'))
        let user = JSON.parse(localStorage.getItem('user'))
        this.name = user[this.userId].name
        this.account = user[this.userId].account
        this.password = user[this.userId].password
        this.phone = user[this.userId].phone
        this.email = user[this.userId].email
        this.address = user[this.userId].address
        
    },
    computed:{
    }
}
</script>

<style lang="scss">
.dataTable{
    width:450px;
    margin:auto;
    padding:10px;
    background-color: rgb(240, 243, 242);
    h1{
        padding-bottom:10px;
        font-size: 26px;
    }
    table{
        margin:auto;
    }
    th,td{
        border:1px solid #000;
        padding:5px;
    }
    th{
        text-align: center;
        background-color: rgb(108, 248, 178);
    }
    td{
        background-color: rgb(196, 253, 201);
    }
}
</style>

