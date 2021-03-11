
<template>
  <div class="hello">
    <div>{{ msg }}</div>
    <div class="checkerboard">
      <!-- <canvas id="Gobang" width="750" height="750" @click="mousedown"></canvas> -->
      <div class="tempC" v-for="(item,i) in 15" :key="i">
        <div class="temp" v-for="(item2,i2) in 15" :key="i2" :id="'id'+item+' '+item2" @click="go(item,item2)"></div>
      </div>
    </div>
    <button @click="reset">重新开始</button>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to 五子棋',
      status:true,//判断落子方
      ifGo:true,//判断是否可落子
      GoneArr:[],//已落子id数组
      blackArr:[],//黑子落子数组
      whiteArr:[],//白子落子数组
      ifWinArr:[],//判断胜利数组
      gameOver:false
    }
  },
  methods:{
    init (){
      //绘制棋盘
      // const canvas = document.getElementById('Gobang');
      // const ctx = canvas.getContext('2d');
      // console.log(ctx);
      // ctx.fillRect(750,750)  
    },
    //落子
    go (item,item2){
      //首先检测是否能落子
      const chessId = 'id'+item+' '+item2;
      const ifGo = this.GoneArr.indexOf(chessId)
      if(ifGo==-1 && this.gameOver!=true){
        //落子
        if(this.status){
          this.blackArr.push(chessId)
        }else{
          this.whiteArr.push(chessId)
        }
        this.GoneArr.push(chessId);//保存已落子的位置，用来判断是否可落子
        //落子完成判断是否胜利
        this.ifWin(item,item2,chessId);
        //更改落子归属方
        this.changePlayer(chessId);
      }
    },
    //判断是否胜利
    ifWin(item,item2,chessId){
      //首先保存最后的落子
      this.ifWinArr=[chessId]
      //确定归属方
      let arr =this.status?this.blackArr:this.whiteArr;
      //其次对八个方向进行判断，如果有连续5个棋子则判定胜利
      //横向判断
      if(!this.gameOver){
        this.leftRight(item,item2,arr,chessId);
      }
      //纵向判断
      if(!this.gameOver){
        this.topBottom(item,item2,arr,chessId);
      }
      //斜向判断
      if(!this.gameOver){
        this.leftBottom(item,item2,arr,chessId);
      }
      //反斜向判断
      if(!this.gameOver){
        this.leftTop(item,item2,arr,chessId);
      }
    },
    //更改落子归属方
    changePlayer(chessId){
      const chessStyle = document.getElementById(chessId).style;
      chessStyle.borderRadius='50%';
      chessStyle.border='none';
      //更改落子的样式
      if(this.status){
        chessStyle.background="black";
      }else{        
        chessStyle.background="white";
        chessStyle.border="1px solid black";
      }
      this.status=!this.status;
    },
    //横向判断
    leftRight(item,item2,arr,chessId){
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+item+' '+(item2+i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+item+' '+(item2-i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      this.ifAndReset(chessId);
    },
    //纵向判断
    topBottom(item,item2,arr,chessId){
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item+i)+' '+item2;
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item-i)+' '+item2;
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          
          break
        }
      }
      this.ifAndReset(chessId);
    },
    //斜向判断
    leftTop(item,item2,arr,chessId){
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item+i)+' '+(item2-i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item-i)+' '+(item2+i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      this.ifAndReset(chessId);
    },
    //反斜向判断
    leftBottom(item,item2,arr,chessId){
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item+i)+' '+(item2+i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      for(let i=1;i<=5;i++){
        let chessIds = 'id'+(item-i)+' '+(item2-i);
        if(arr.indexOf(chessIds)!=-1){
          this.ifWinArr.push(chessIds);
        }else{
          break
        }
      }
      this.ifAndReset(chessId);
    },
    //判断及重置
    ifAndReset(chessId){
      if(this.ifWinArr.length>=5){
        let player =this.status?'黑子':'白子';
        alert('恭喜！'+player+'获得胜利');
        this.gameOver=true;
        return
      }else{
        //横向和纵向都不符合时重置
        this.ifWinArr=[chessId]
      }
    },
    //重新开始按钮
    reset(){
      window.location.reload()
    }
  },
  mounted(){
    console.log('加载完成');
    // this.init();
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#Gobang{
    background-color: #ccc;
}
.checkerboard{
  margin: 0 auto;
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  background-image: url('../assets/map.jpg');
  background-position: -6px -7px;
}
.tempC{
  display: flex;
}
.temp{
  width: 20px;
  height: 20px;
  /* background-color: #ccc; */
  box-sizing: border-box;
}
</style>
