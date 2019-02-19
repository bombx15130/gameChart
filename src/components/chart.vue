<template>
  <div class="gameChartWrap">
    <div class="chartTitle">
      XXX基本走勢
    </div>
    <button @click="rand">30random records</button>
    <input type="checkbox" id="checked" value="checked" v-model="checkedNames">隨機作廢
    <div class="tableBlock">
      
      <table>
        
        <thead>
          <tr>
            <th rowspan="2">期号</th>
            <th rowspan="2">开奖号码</th>
            <th colspan="10">万位</th>
            <th colspan="10">仟位</th>
            <th colspan="10">百位</th>
            <th colspan="10">十位</th>
            <th colspan="10">个位</th>
          </tr>
          <tr>
            <th v-for="n in numbers" class="col">{{ n }}</th>
            <th v-for="n in numbers" class="col">{{ n }}</th>
            <th v-for="n in numbers" class="col">{{ n }}</th>
            <th v-for="n in numbers" class="col">{{ n }}</th>
            <th v-for="n in numbers" class="col">{{ n }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="w in winNumber" :key="w" >
            <td>{{w.series}}</td>
            <td>{{w.numbers}}</td>
            <td v-for="n in numbers" :class="['col',n==w.numbers.split(',')[0]?'winN':'']">{{ n==w.numbers.split(',')[0]?n:w.miss[0][n] }}</td>
            <td v-for="n in numbers" :class="['col',n==w.numbers.split(',')[1]?'winN':'']">{{ n==w.numbers.split(',')[1]?n:w.miss[1][n] }}</td>
            <td v-for="n in numbers" :class="['col',n==w.numbers.split(',')[2]?'winN':'']">{{ n==w.numbers.split(',')[2]?n:w.miss[2][n] }}</td>
            <td v-for="n in numbers" :class="['col',n==w.numbers.split(',')[3]?'winN':'']">{{ n==w.numbers.split(',')[3]?n:w.miss[3][n] }}</td>
            <td v-for="n in numbers" :class="['col',n==w.numbers.split(',')[4]?'winN':'']">{{ n==w.numbers.split(',')[4]?n:w.miss[4][n] }}</td>
          </tr>
          <tr>
            <th>出現總次數</th>
            <th></th>
            <td v-for="c in totalWinNumber">{{ c }}</td>
          </tr>
          <tr>
            <th>最大遺漏</th>
            <th></th>
            <td v-for="m in missMaxC" >{{ m }}</td>
          </tr>
        </tbody>
      </table>
      <canvas id="canvas"></canvas>
    </div>
  </div>
  
</template>

<script>

export default {
  data () {
    return {
      numbers:[0,1,2,3,4,5,6,7,8,9],
      checkedNames:null,
      winNumber:null,
      missMax:null,
      totalWinNumber:[]
    }
  },
  created:function(){
     for(let i =0;i<50;i++){
       this.totalWinNumber.push(0)
     }
  },
  computed:{
    missMaxC(){
      return this.missMax.flat()
    },
  },
  mounted:function(){
          this.rand()
        this.$nextTick(() => {
          
          this.getData()
        })
  },
  components:{
    
  },
  methods:{
    getData(){
      // 取得各點座標
      const table = document.querySelector('table')
      const tbody =document.querySelector('tbody')
        const trs = tbody.querySelectorAll('tr')
        const locArr = []
        const locNumArr= []
        for(let i =0; i<trs.length; i++){
          const tds = trs[i].querySelectorAll('td')
          
          for(let j=2; j<tds.length;j++){
            if(tds[j].classList.length ===2){
              // locNumArr.push([tds[j].offsetLeft +15,tds[j].offsetTop-table.offsetTop+tds[j].clientHeight/2])
              locNumArr.push([tds[j].offsetLeft + tds[j].offsetWidth / 2, tds[j].offsetTop - table.offsetTop + tds[j].clientHeight / 2])
            }
          }
          
        }
        locArr.push(locNumArr)
        // const locArrN = locArr.flat().filter(value => value != '')
      console.log(locArr)
    //  畫圖
    const canvas = document.querySelector('#canvas')
    canvas.setAttribute('width',table.clientWidth)
    canvas.setAttribute('height',table.clientHeight)

    if(canvas.getContext){
      const ctx = canvas.getContext("2d");
      console.log(locArr.length)
      for(let i =0;i<locArr[0].length;i++){
        ctx.fillStyle = (i % 5)%2 == 1 ? '#54b8fd' : '#fea4a4'
        ctx.strokeStyle = (i % 5)%2 == 1 ? '#54b8fd' : '#fea4a4'
        ctx.lineCap = 'round'
        ctx.lineWidth = 2
        
        if(i<5){
          ctx.beginPath()
          ctx.moveTo(locArr[0][i][0], locArr[0][i][1])
          // circle
          ctx.arc(locArr[0][i][0], locArr[0][i][1], 12, 0, 2*Math.PI, true)
          ctx.fill()
        }else{
          ctx.beginPath()
            ctx.moveTo(locArr[0][i-5][0], locArr[0][i-5][1])
            ctx.lineTo(locArr[0][i][0], locArr[0][i][1])
            ctx.closePath()
            ctx.stroke()
            // circle
            ctx.arc(locArr[0][i][0], locArr[0][i][1], 12, 0, 2*Math.PI, true)
            ctx.fill()
        }
        
      }
    }
    },
    rand(){
      // 把出現總數的陣列清空避免重複計算
        this.totalWinNumber = this.totalWinNumber.map(x=>x=0)

      /* 測試塞的假資料 */
        const data = []
        
        for (let i = 1; i <= 30; i++) {
          // 塞中獎的數字
          let numbers = []
          if(this.checkedNames === true && getRandomInt(0, 10) > 9){
            numbers = ['X','X','X','X','X']
          }else{
            for (let j = 0; j < 5; j++) {
              numbers.push(getRandomInt(0, 9))
            }
          }
          
          // 塞遺漏數字陣列
          const missN =[]
          for(let k=0;k<numbers.length;k++){
            const temp = []
            for(let s=0;s<10;s++){
              if(numbers[k]==='X' || s !== parseInt(numbers[k],10) ){
                if(i===1){
                  temp[s]=1
                }else{
                  temp[s]=data[i-2].miss[k][s]+1
                }
              }else{
                temp[s]=0
                this.totalWinNumber[(k*10)+s]+=1
              }
            }
            missN.push(temp)
          }

          // 判斷最大遺漏(v1)
          if(i===1){
            this.missMax = JSON.parse(JSON.stringify(missN))
          }else{
            for(let w=0; w<5;w++){
              for(let y=0; y<10 ;y++){
                  this.missMax[w][y] = Math.max(this.missMax[w][y],missN[w][y])
                }
              }
          }

          // 隨機作廢
          // if (this.checkedNames === true && getRandomInt(0, 10) > 9) {
          //   data.push({ series: 1000000000 + i, numbers: 'X,X,X,X,X',miss:missN })
          //   continue
          // }
          data.push({ series: 1000000000 + i, numbers: numbers.join(','),miss:missN})
        }

        // 隨機產生0~9的數字
        function getRandomInt(min, max) {
          return Math.floor(Math.random() * (max - min + 1)) + min
        } 
        
        this.winNumber = data.reverse()
        // console.log(this.winNumber)
        // console.log(this.missMax)
        // console.log(this.totalWinNumber)

        // 先執行rand()取得資料再畫圖
        this.$nextTick(() => {
          this.getData()
        })
          
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.gameChartWrap{
  // background-color: #CCC;
  width:95%;
  margin:auto;
  padding-bottom: 43%;
  box-sizing: border-box;
  .chartTitle{
    padding-bottom:20px;
  }
  .tableBlock{
    position: relative;
    #canvas{
      z-index: 0;
      position:absolute;
      // background-color: aqua;
      left:0;
      top:0;
    }
    table{
      // z-index: 100;
      width:100%;
      z-index: 999;
      position: absolute;
      left:0;
      top:0;
      thead{
        // background-color: #888;
        border-bottom:2px solid #000;
        tr:first-child{
          th{
            padding:10px 0px;
          }
          
        }
        .col{
          border:1px solid #000;
          color:#000;
          padding:5px;
        }
      }
      tbody{
        tr{
          border-bottom:1px solid #ccc;
          border-style: dotted ;
          .col{
            color:#000;
            padding:10px 0px;
          }
          .winN{
            color:#FFF;
          }
        }
        
      }
      
    }
  }
}
</style>
