<template>
  <div class="home">
    <!-- 月 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('months')" ref="months">
        <span
          v-for="(item, index) in monthTexts"
          :key="item"
          :class="{'active': index === currentMonth - 1}"
          :style="spanStyle(boxSize.months, monthTexts, index)"
        >{{ item }}</span>
        <i class="circle-dot"></i>
      </div>
    </div>
    <!-- 日 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('days')" ref="days">
        <span
          v-for="(item, index) in dayTexts"
          :key="item"
          :class="{'active': index === currentDay - 1}"
          :style="spanStyle(boxSize.days, dayTexts, index)"
        >{{ item }}</span>
      </div>
    </div>
    <!-- 小时 -->
    <div class="box-wrapper">
      <div class="circle-box" :style="boxStyle('hours')" ref="hours">
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
      <div class="circle-box" :style="boxStyle('minutes')" ref="minutes">
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
      <div class="circle-box" :style="boxStyle('seconds')" ref="seconds">
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
        '一号', '二号', '三号', '四号', '五号', '六号', '七号', '八号', '九号', '十号',
        '十一号', '十二号', '十三号', '十四号', '十五号', '十六号', '十七号', '十八号', '十九号', '二十号', 
        '二十一号', '二十二号', '二十三号', '二十四号', '二十五号', '二十六号', '二十七号', '二十八号', '二十九号', '三十号', , '三十一号'
      ],
      hourTexts: [
        '一点', '二点', '三点', '四点', '五点', '六点', '七点', '八点', '九点', '十点', '十一点', '十二点',
        '十三点', '十四点', '十五点', '十六点', '十七点', '十八点', '十九点', '二十点', '二十一点', '二十二点', '二十三点', '二十四点'
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
        months: 100,
        days: 200,
        hours: 300,
        minutes: 420,
        seconds: 550
      },
      // 定义当前时间
      currentMonth: 0, // 月
      currentDay: 0, // 日
      currentHours: 0, // 时
      currentMinutes: 0, // 分
      currentSeconds: 0, // 秒
      // 定义当前各个旋转角度
      monthDeg: 0,
      dayDeg: 0,
      hourDeg: 0,
      minuteDeg: 0,
      secondDeg: 0,
      // 定时器
      timer: null,
      isFullScreen: false,
      isSound: false
    }
  },

  created() {
    const isSound = localStorage.getItem('is_sound')
    this.isSound = JSON.parse(isSound) || false
  },

  mounted() {
    this.$nextTick(() => {
      this.init()
    })
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
      this.currentMonth = month
      this.currentDay = day
      this.currentHours = hours
      this.currentMinutes = minutes
      this.currentSeconds = seconds
      // 角度
      this.monthDeg = (this.currentMonth - 1) * this.getPerDeg(this.monthTexts)
      this.dayDeg = (this.currentDay - 1) * this.getPerDeg(this.dayTexts)
      this.hourDeg = (this.currentHours - 1) * this.getPerDeg(this.hourTexts)
      this.minuteDeg = this.currentMinutes * this.getPerDeg(this.minuteTexts)
      this.secondDeg = this.currentSeconds * this.getPerDeg(this.secondTexts)
      // 旋转到当前位置
      this.handleRotate('months', this.monthDeg)
      this.handleRotate('days', this.dayDeg)
      this.handleRotate('hours', this.hourDeg)
      this.handleRotate('minutes', this.minuteDeg)
      this.handleRotate('seconds', this.secondDeg)
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

    boxStyle(key) {
      return {
        width: this.boxSize[key] + 'px',
        height: this.boxSize[key] + 'px'
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

    handleRotate(refName, deg) {
      if (this.isSound) {
        this.$refs['audio'].play()
      } else {
        this.$refs['audio'].pause()
      }
      this.$refs[refName].style.transform = `rotate(-${deg}deg)`
    },

    runClock() {
      const d = new Date()
      const year = d.getFullYear() // 年
      const month = d.getMonth() + 1 // 月
      const day = d.getDate() // 日
      const hours = d.getHours() // 时
      const minutes = d.getMinutes() // 分
      const seconds = d.getSeconds() // 秒
      if (this.currentMonth !== month) {
        this.currentMonth = month
        this.monthDeg += this.getPerDeg(this.monthTexts)
        this.handleRotate('months', this.monthDeg)
      }
      if (this.currentDay !== day) {
        this.currentDay = day
        this.dayDeg += this.getPerDeg(this.dayTexts)
        this.handleRotate('days', this.dayDeg)
      }
      if (this.currentHours !== hours) {
        this.currentHours = hours
        this.hourDeg += this.getPerDeg(this.hourTexts)
        this.handleRotate('hours', this.hourDeg)
      }
      if (this.currentMinutes !== minutes) {
        this.currentMinutes = minutes
        this.minuteDeg += this.getPerDeg(this.minuteTexts)
        this.handleRotate('minutes', this.minuteDeg)
      }
      this.currentSeconds = seconds
      this.secondDeg += this.getPerDeg(this.secondTexts)
      this.handleRotate('seconds', this.secondDeg)
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
  background-color: #232323;
  color: #71767D;
  position: relative;
  min-width: 800px;
  min-height: 600px;
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
  font-size: 13px;
  // display: inline-block;
  // width: 50px;
  // height: 20px;
  // line-height: 20px;
  // border: 1px solid #6eb0b2;
  box-sizing: border-box;
  &.active {
    color: #6eb0b2;
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
