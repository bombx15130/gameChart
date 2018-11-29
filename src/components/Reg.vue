<template>
    <div class="reg">
        <h2>註冊</h2>
        <table>
            <tr>
                <th>帳號</th><td><input type="text" placeholder="帳號"></td>
            </tr>
            <tr>
                <th>密碼</th><td><input type="password" placeholder="密碼"></td>
            </tr>
            <tr>
                <th>確認密碼</th><td><input type="password" placeholder="再次輸入密碼"></td>
            </tr>
            <tr>
                <th>電子信箱</th><td><input type="email" placeholder="example@gmail.com"></td>
            </tr>
            <tr>
                <th>姓名</th><td><input type="text" placeholder="姓名"></td>
            </tr>
            <tr>
                <th>手機</th><td><input type="tel" placeholder="手機號碼"></td>
            </tr>
            <tr>
                <th>地址</th><td><input type="text" placeholder="地址"></td>
            </tr>
            <tr>
                <th>會員條款</th><td><input type="checkbox">我已詳閱會員條款</td>
            </tr>
        </table>
        <button class="submitBtn">送出資料</button>
    </div>
</template>

<script>
export default {
    data(){
        return{
        }
    }
}
</script>

<style lang="scss">
.reg{
    width:400px;
    border:1px solid rgba(37, 6, 6, 0.3);
    margin:auto;
    box-sizing: border-box;
    background: rgba(250, 250, 250, 0.92);
    border-radius: 5px;
    box-shadow: 0px 4px 3px rgba(106, 131, 134, 0.1);
    h2{
        font-size:20px;
        font-family: $fontF;
        padding:20px 0px;
    }
    table{
        width:90%;
        border-collapse: separate;
        margin:auto;
        padding:10px;
        box-sizing: border-box;
        th,td{
            padding:5px;
        }
        th{
            border-bottom:1px solid #FFF;
            background-color: #DDD;
        }
        input{
            width:100%;
            padding:3px;
        }
        input[type=checkbox]{
            width:20px;
        }
        td:last-child{
            text-align: left;
        }
    }
    .submitBtn{
        display: block;
        margin:auto;
        background-color: #285f8f; 
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
        margin:20px;
    }
}
</style>

