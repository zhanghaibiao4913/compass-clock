<template>
  <div class="home">
    <!-- 月 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('months', monthsDeg)" ref="months">
        <span
          v-for="(item, index) in monthTexts"
          :key="item"
          :class="{'active': index === currentMonths - 1}"
          :style="spanStyle(boxSize.months, monthTexts, index)"
        >{{ item }}</span>
        <i class="circle-dot"></i>
      </div>
    </div>
    <!-- 日 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('days', daysDeg)" ref="days">
        <span
          v-for="(item, index) in dayTexts"
          :key="item"
          :class="{'active': index === currentDays - 1}"
          :style="spanStyle(boxSize.days, dayTexts, index)"
        >{{ item }}</span>
      </div>
    </div>
    <!-- 小时 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('hours', hoursDeg)" ref="hours">
        <span
          v-for="(item, index) in hourTexts"
          :key="item"
          :class="{'active': index === currentHours - 1}"
          :style="spanStyle(boxSize.hours, hourTexts, index)"
        >{{ item }}</span>
      </div>
    </div>
    <!-- 分 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('minutes', minutesDeg)" ref="minutes">
        <span
          v-for="(item, index) in minuteTexts"
          :key="item"
          :class="{'active': index === currentMinutes}"
          :style="spanStyle(boxSize.minutes, minuteTexts, index)"
        >{{ item }}</span>
      </div>
    </div>
    <!-- 秒 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('seconds', secondsDeg)" ref="seconds">
        <span
          v-for="(item, index) in secondTexts"
          :key="item"
          :class="{'active': index === currentSeconds}"
          :style="spanStyle(boxSize.seconds, secondTexts, index)"
        >{{ item }}</span>
      </div>
    </div>
    <!-- 转动音乐 -->
    <audio ref="audio" :src="require('@/assets/music.mp3')"></audio>
    <!-- 小按钮 -->
    <div class="action-btn-bar">
      <img
        :src="isSound ? require('@/assets/open-sound.png') : require('@/assets/close-sound.png')"
        style="width: 16px; height: 16px; margin-right: 10px;"
        :title="isSound ? '关闭声音' : '打开声音'"
        @click="toggleSound"
      />
      <img
        :src="isFullScreen ? require('@/assets/exit-full-screen.png') : require('@/assets/full-screen.png')"
        style="width: 20px; height: 20px;"
        :title="isFullScreen ? '退出全屏' : '全屏'"
        @click="toggleFullScreen"
      />
    </div>
  </div>
</template>

<script>
import screenfull from 'screenfull'

export default {
  name: 'Home',

  data() {
    return {
      monthTexts: [
        '一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月'
      ],
      dayTexts: [
        '一日', '二日', '三日', '四日', '五日', '六日', '七日', '八日', '九日', '十日',
        '十一日', '十二日', '十三日', '十四日', '十五日', '十六日', '十七日', '十八日', '十九日', '二十日', 
        '二十一日', '二十二日', '二十三日', '二十四日', '二十五日', '二十六日', '二十七日', '二十八日', '二十九日', '三十日', , '三十一日'
      ],
      hourTexts: [
        '一时', '二时', '三时', '四时', '五时', '六时', '七时', '八时', '九时', '十时', '十一时', '十二时',
        '十三时', '十四时', '十五时', '十六时', '十七时', '十八时', '十九时', '二十时', '二十一时', '二十二时', '二十三时', '二十四时'
      ],
      minuteTexts: [
        '零零', '一分', '二分', '三分', '四分', '五分', '六分', '七分', '八分', '九分', '十分',
        '十一分', '十二分', '十三分', '十四分', '十五分', '十六分', '十七分', '十八分', '十九分', '二十分',
        '二十一分', '二十二分', '二十三分', '二十四分', '二十五分', '二十六分', '二十七分', '二十八分', '二十九分', '三十分',
        '三十一分', '三十二分', '三十三分', '三十四分', '三十五分', '三十六分', '三十七分', '三十八分', '三十九分', '四十分',
        '四十一分', '四十二分', '四十三分', '四十四分', '四十五分', '四十六分', '四十七分', '四十八分', '四十九分', '五十分',
        '五十一分', '五十二分', '五十三分', '五十四分', '五十五分', '五十六分', '五十七分', '五十八分', '五十九分'
      ],
      secondTexts: [
        '零零', '一秒', '二秒', '三秒', '四秒', '五秒', '六秒', '七秒', '八秒', '九秒', '十秒',
        '十一秒', '十二秒', '十三秒', '十四秒', '十五秒', '十六秒', '十七秒', '十八秒', '十九秒', '二十秒',
        '二十一秒', '二十二秒', '二十三秒', '二十四秒', '二十五秒', '二十六秒', '二十七秒', '二十八秒', '二十九秒', '三十秒',
        '三十一秒', '三十二秒', '三十三秒', '三十四秒', '三十五秒', '三十六秒', '三十七秒', '三十八秒', '三十九秒', '四十秒',
        '四十一秒', '四十二秒', '四十三秒', '四十四秒', '四十五秒', '四十六秒', '四十七秒', '四十八秒', '四十九秒', '五十秒',
        '五十一秒', '五十二秒', '五十三秒', '五十四秒', '五十五秒', '五十六秒', '五十七秒', '五十八秒', '五十九秒'
      ],
      boxSize: {
        months: 80,
        days: 180,
        hours: 320,
        minutes: 440,
        seconds: 580
      },
      // 定义当前时间
      currentMonths: 0, // 月
      currentDays: 0, // 日
      currentHours: 0, // 时
      currentMinutes: 0, // 分
      currentSeconds: 0, // 秒
      monthsDeg: 0,
      daysDeg: 0,
      hoursDeg: 0,
      minutesDeg: 0,
      secondsDeg: 0,
      // 定时器
      timer: null,
      isFullScreen: false,
      isSound: false
    }
  },

  created() {
    const val = localStorage.getItem('is_sound')
    this.isSound = JSON.parse(val) || false
  },

  mounted() {
    this.init()

    // 解决浏览器切换回来后出现定时旋转错误情况
    document.onvisibilitychange = () => {
      // console.log(document.visibilityState)
      clearInterval(this.timer)
      if (document.visibilityState === 'visible') {
        this.init()
      }
    }
  },

  methods: {
    init() {
      const d = new Date()
      const year = d.getFullYear() // 年
      const month = d.getMonth() + 1 // 月
      const day = d.getDate() // 日
      const hours = d.getHours() // 时
      const minutes = d.getMinutes() // 分
      const seconds = d.getSeconds() // 秒
      // 获取当前月份天数
      const monthDays = new Date(year, month, 0).getDate()
      // 删除多余的天数
      this.dayTexts = this.dayTexts.slice(0, monthDays)
      // 当前时间
      this.currentMonths = month
      this.currentDays = day
      this.currentHours = hours
      this.currentMinutes = minutes
      this.currentSeconds = seconds
      // 角度
      this.monthsDeg = (this.currentMonths - 1) * this.getPerDeg(this.monthTexts)
      this.daysDeg = (this.currentDays - 1) * this.getPerDeg(this.dayTexts)
      this.hoursDeg = (this.currentHours - 1) * this.getPerDeg(this.hourTexts)
      this.minutesDeg = this.currentMinutes * this.getPerDeg(this.minuteTexts)
      this.secondsDeg = this.currentSeconds * this.getPerDeg(this.secondTexts)
      
      // 设置定时器
      this.timer = setInterval(() => {
        this.runClock()
      }, 1000)
      // 记得清除定时器
      this.$once('hook:beforeDestroy', () => {
        clearInterval(this.timer)
      })
    },

    // 获取平均角度
    getPerDeg(texts) {
      return 360 / texts.length
    },

    boxStyle(key, deg) {
      return {
        width: this.boxSize[key] + 'px',
        height: this.boxSize[key] + 'px',
        transform: `rotate(-${deg}deg)`
      }
    },

    spanStyle(size, datas, i) {
      const long = size / 2 // 半径
      const deg = this.getPerDeg(datas) // 元素之间的间隔度数
      const angle = i * deg // 角度
      const { a, b } = this.hypotenuse(long, angle)
      const rotateDeg = deg * i
      return {
        top: a + long + 'px',
        left: b + long + 'px',
        transform: `rotate(${rotateDeg}deg)`,
        transformOrigin: '0 0'
      }
    },

    // 已知角度和斜边，求直角边
    hypotenuse(long, angle) {
      // 获得弧度
      let radian = 2 * Math.PI / 360 * angle
      return {
        a: Math.sin(radian) * long, // 邻边
        b: Math.cos(radian) * long // 对边
      }
    },

    playMusic() {
      if (this.$refs['audio']) {
        if (this.isSound) {
          this.$refs['audio'].play()
        } else {
          this.$refs['audio'].pause()
        }
      }
    },

    runClock() {
      const d = new Date()
      const year = d.getFullYear() // 年
      const month = d.getMonth() + 1 // 月
      const day = d.getDate() // 日
      const hours = d.getHours() // 时
      const minutes = d.getMinutes() // 分
      const seconds = d.getSeconds() // 秒
      if (this.currentMonths !== month) {
        this.currentMonths = month
        this.monthsDeg += this.getPerDeg(this.monthTexts)
      }
      if (this.currentDays !== day) {
        this.currentDays = day
        this.daysDeg += this.getPerDeg(this.dayTexts)
      }
      if (this.currentHours !== hours) {
        this.currentHours = hours
        this.hoursDeg += this.getPerDeg(this.hourTexts)
      }
      if (this.currentMinutes !== minutes) {
        this.currentMinutes = minutes
        this.minutesDeg += this.getPerDeg(this.minuteTexts)
      }
      this.currentSeconds = seconds
      this.secondsDeg += this.getPerDeg(this.secondTexts)
      this.playMusic()
    },

    // 切换声音开关
    toggleSound() {
      this.isSound = !this.isSound
      localStorage.setItem('is_sound', this.isSound)
    },

    // 全屏切换
    toggleFullScreen() {
      if (screenfull.isEnabled) {
        screenfull.toggle()
        this.isFullScreen = !this.isFullScreen
      } else {
        console.error('你的浏览器不支持全屏')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  height: 100%;
  width: 100%;
  background-color: #000000;
  color: #71767D;
  position: relative;
  min-width: 800px;
  min-height: 660px;
  padding: 20px 0;
  overflow: hidden;
}

.box-wrapper {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.circle-box {
  position: relative;
  transition: transform 0.4s ease-in-out;
  // border: 1px solid red;
  // box-sizing: border-box;
  // border-radius: 50%;
  // .circle-dot {
  //   position: absolute;
  //   left: 50%;
  //   top: 50%;
  //   transform: translate(-50%, -50%);
  //   display: inline-block;
  //   width: 8px;
  //   height: 8px;
  //   background-color: red;
  //   border-radius: 50%;
  // }
}

.circle-box span {
  position: absolute;
  white-space: nowrap;
  font-size: 14px;
  // display: inline-block;
  // width: 50px;
  // height: 20px;
  // line-height: 20px;
  // border: 1px solid #6eb0b2;
  box-sizing: border-box;
  &.active {
    color: #fff;
  }
}

.action-btn-bar {
  position: absolute;
  right: 20px;
  top: 20px;
  display: flex;
  align-items: center;
  img {
    cursor: pointer;
  }
}
</style>
