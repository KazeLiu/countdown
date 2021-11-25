/*
* 翻牌时钟
* @author： 兔子先生
* @createDate: 2019-11-24
*/
<template>
  <div id="app">
    <div class="title">距离{{ 'xxxxxxxxx' }}还有：</div>
    <div class="FlipClock">
      <Flipper :key="'day_'+index" :text="item" v-for="(item,index) in dayList" :max="9"/>
      <em>天</em>
      <Flipper :text="hourList[0]" :max="6"/>
      <Flipper :text="hourList[1]" :max="9"/>
      <em>小时</em>
      <Flipper :text="minutesList[0]" :max="6"/>
      <Flipper :text="minutesList[1]" :max="9"/>
      <em>分钟</em>
      <Flipper :text="secondsList[0]" :max="6"/>
      <Flipper :text="secondsList[1]" :max="9"/>
      <em>秒</em>
    </div>
  </div>
</template>

<script>
import Flipper from './Flipper'

export default {
  name: 'FlipClock',
  data() {
    return {
      timer: null,
      dayList: [],
      hourList: [],
      minutesList: [],
      secondsList: [],
    }
  },
  components: {
    Flipper
  },
  methods: {
    // 初始化数字
    init() {
      setInterval(() => {
        let now = new Date().getTime()
        let stop = new Date('2025-12-31 23:59:59').getTime()
        let usedTime = stop - now;
        let days = Math.floor(usedTime / (24 * 3600 * 1000)).toString();
        //计算出小时数
        let leave1 = usedTime % (24 * 3600 * 1000);    //计算天数后剩余的毫秒数
        let hours = this.padLeftZero(Math.floor(leave1 / (3600 * 1000)));
        //计算相差分钟数
        let leave2 = leave1 % (3600 * 1000);        //计算小时数后剩余的毫秒数
        let minutes = this.padLeftZero(Math.floor(leave2 / (60 * 1000)));

        let leave3 = leave2 % (60 * 1000)      //计算分钟数后剩余的毫秒数
        let seconds = this.padLeftZero(Math.round(leave3 / 1000));

        this.dayList = days.split('');
        this.hourList = hours.split('');
        this.minutesList = minutes.split('');
        this.secondsList = seconds.split('');
      }, 1000)
      // setInterval(() => {
      //   let now = new Date()
      //   this.dayList = now.getDay().toString().split('');
      //   this.hourList = now.getHours().toString().split('');
      //   this.minutesList = now.getMinutes().toString().split('');
      //   this.secondsList = this.padLeftZero(now.getSeconds().toString()).split('');
      // }, 1000)
    },
    // 日期时间补零
    padLeftZero(str) {
      return ('00' + str).substr(str.toString().length)
    }
  },
  mounted() {
    this.init()
  }
}
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}

#app {
  height: 100vh;

}

.title {
  width: 100%;
  text-align: center;
  font-size: 5vw;
  color: #AA071C;
  margin-bottom: 30px;
}

.FlipClock {
  text-align: center;
  user-select: none;

  .M-Flipper {
    margin: 0 6px;
  }

  em {
    display: inline-block;
    line-height: 27vh;
    font-size: 25px;
    font-style: normal;
    vertical-align: top;
    color: #D1413F;
    margin-left: 5px;
    margin-right: 30px;
    height: 15vh;
  }
}
</style>
