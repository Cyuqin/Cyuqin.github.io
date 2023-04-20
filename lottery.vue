<template>
  <div class='main' oncontextmenu="return false" ondragstart="return false;">
    <h2 class='times'>抽奖剩余 {{ luckClick }}次 </h2>
    <div class='rotate'>
      <!--转盘图片-->
      <img 
        src="@/assets/dzp-14.png" 
        class='dish'
        :style='{transform:rotate_deg,transition:rotate_transition}'
      />
      <!--指针图片-->
      <img src="@/assets/click.png" class='pointer' @click='start'/>
    </div>
  </div>
</template>

<script>
export default{
  data () {
    return {
      luckClick:3,//抽奖剩余次数
      rotate_deg:0,//转盘的旋转角度
      rotate_transition:'transform 3s ease-in-out',//初始化过渡属性控制
      isClick:true,
    }
  },
  methods:{
    //触发开始抽奖按钮
    start(){
      if( this.luckClick == 0 ){
        alert('抽奖次数已用完~');
        return;
      }
      this.rotating();
    },
    //转盘抽奖的业务逻辑
    rotating(){
      if( !this.isClick ) return;
      this.isClick = false;
      this.rotate_transition = 'transform 3s ease-in-out';
      
      //减少抽奖剩余次数
      this.luckClick--;
      //默认多旋转5圈
      let rand_circle = 5;
      //设置抽奖的概率
      let winIndex = this.set();

      let randomDeg = 360 - winIndex * 45;

      let deg = rand_circle * 360 + randomDeg;
      //最终旋转
      this.rotate_deg = "rotate("+deg+"deg)";

      let that = this;
      setTimeout(function(){
        that.isClick = true;
        that.rotate_deg = "rotate("+0+"deg)";
        that.rotate_transition = '';
        if( winIndex == 5 ){
          alert('谢谢参与');
        }else if( winIndex == 6 ){
          alert(' 恭喜获得5元现金');
        }else if( winIndex == 4 ){
          alert(' 恭喜获得10元现金');
        }else if( winIndex == 0 ){
          alert(' 恭喜获得20元现金');
        }else if( winIndex == 2 ){
          alert(' 恭喜获得50元现金');
        }else if( winIndex == 1 ){
          alert(' 恭喜获得幸运奖~相机一个');
        }else if( winIndex == 3 ){
          alert(' 恭喜获得幸运奖~手环一个');
        }else if( winIndex == 7 ){
          alert(' 恭喜获得幸运奖~手表一个');
        } 
      },3500)
    },
    //设置抽奖概率
    set(){
      let winIndex;
      let rand = Math.random();
      if( rand < 0.3 ) winIndex = 5;
      else if( rand < 0.55 ) winIndex = 6;
      else if( rand < 0.75 ) winIndex = 4;
      else if( rand < 0.85 ) winIndex = 0;
      else if( rand < 0.92 ) winIndex = 2;
      else if( rand < 0.97 ) winIndex = 1;
      else if( rand < 0.99 ) winIndex = 3;
      else if( rand == 0.99 ) winIndex = 7;
      return winIndex;
    }
  }
}
</script>

<style>
*{ margin:0;padding:0; }
.main{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  text-align: center;
}  
.rotate{
  position: relative;
  width: 540px;
  height: 540px;
}
.pointer{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  width: 139px;
  height: 203px;
}
</style>



