<template>
  <div id="app">
      <swiper v-if="pageType == 0" ref="mySwiper" :options="swiperOptions">
        <swiper-slide>
          <word-cloud :type="genderRandom?'male':'female'"></word-cloud>
        </swiper-slide>
        <swiper-slide>
          <word-cloud :type="!genderRandom?'male':'female'"></word-cloud>
        </swiper-slide>
      </swiper>

      <div v-if="pageType == 1" :style="{width:'100vw',height:'100vh',background:'url('+require('./static/jia.png')+')'}">
      </div>

      <div v-if="pageType == 2" style="margin-left:30px">
        <h1>404 Not Found</h1>
        <p>The requested resource was not found on this server.</p>
      </div>
  </div>
</template>

<script>
import wordCloud from './components/wordCloud'

// 修改标题
document.title = 'There Is No Wall'

// 隐藏滑动条
document.body.parentNode.style.overflowY = "hidden";
document.body.parentNode.style.overflowX = "hidden";

// 导入swiper
import { Swiper as SwiperClass, Mousewheel} from 'swiper/core'
import getAwesomeSwiper from 'vue-awesome-swiper/dist/exporter'
const { Swiper, SwiperSlide } = getAwesomeSwiper(SwiperClass)
SwiperClass.use([Mousewheel])

export default {
  name: 'App',
  data(){
    return{
      pageType: undefined,
      // 性别随机
      genderRandom: (Math.random(1)-0.5) > 0,
      // swiper配置
      swiperOptions: {
        mousewheel:true,
        mousewheelControl:true,
        direction:'vertical',
        height: window.innerHeight,
        width: window.innerWidth
      }
    }
  },
  computed:{
    swiper() {
      return this.$refs.mySwiper.$swiper
    }
  },
  components: {
    Swiper,
    SwiperSlide,
    wordCloud
  },
  mounted(){
    // 前两次打开，不显示404或夹图
    let openTimes = Number(localStorage.getItem('openTimes'))
    if(openTimes){
      openTimes += 1
    }else{
      openTimes = 1
    }
    localStorage.setItem('openTimes', openTimes)
    if(openTimes < 3){
      this.pageType = 0
      return
    }
    // 第三次及以后打开，随机数判断显示什么页面
    let r = Math.random(1)
    if(r < 0.7) {
      this.pageType = 0
    }
    else if(r < 0.85){
      this.pageType = 1
      alert('你查看的东西不存在')
    }
    else{
      this.pageType = 2
    }
  }
}
</script>

<style lang='scss'>
@font-face {
  font-family: 'handfont';
  src: url('static/喜鹊在山林.TTF');
  font-weight: normal;
  font-style: normal;
}

body{
  margin:0px;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
