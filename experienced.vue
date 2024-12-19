<!--author: yezi0507-->
<!--感谢billbill@山羊的前端小窝提供的思路-->
<!--有用点个star喵-->
<template>
<div class="shell" id="shell">
  <h1>标题</h1>
  <div class="header">
    <h3 class="subtitle">副标题</h3>
  </div>
  <div class="timeline">
    <div  v-for="(item, index) in items" :key="item.year" :class="['item', { 'even-item': index % 2 === 0 }]" :data-text="item['data-text'] "  ref="timelineItem">
      <div class="content">
      <img :src="item.img" class="img">
      <h2 class="content-title">{{ item.year }}</h2> <!-- 年份 -->
      <p class="content-desc">
        {{ item.desc }} <!-- 留言 -->
      </p>
     </div>
   </div>
  </div>
</div>
<div class="footer"><a>页脚</a>
</div>
</template>
<script>
export default {
  data () {
    return {
      timelineId: 'shell',
      timelineElement: null, // 用于缓存 timeline 的 DOM 引用
      activeIndex: 0,
      /* 存放你自己的数据*/
      items: [
        {
          'data-text': '嗡嗡嗡',
          img: require('@/assets/image/1.jpg'),
          year: 2015,
          desc: '加油'
        }
    }
  },
  mounted () {
    // 当组件挂载后，可以在这里操作 DOM
    this.timelineElement = document.getElementById(this.timelineId)
    window.addEventListener('scroll', this.handleScroll)
    // 设置第一个项目为 active
    if (this.timelineElement) {
      const items = this.timelineElement.querySelectorAll('.item')
      if (items && items.length > 0) {
        items[0].classList.add('item--active')

        // 设置时间轴背景图片为第一个项目的图片
        // 因为我不需要所以注释掉了
        /* const firstImg = items[0].querySelector('.img')
        if (firstImg) {
          const imgSrc = firstImg.getAttribute('src')
          this.timelineElement.style.backgroundImage = `url(${imgSrc})`
        }
        */
      }
    }
  },
  beforeUnmount () {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll () {
      const pos = window.scrollY
      this.items.forEach((item, index) => {
        const itemElement = this.$refs.timelineItem[index]
        const min = itemElement.offsetTop
        const max = min + itemElement.offsetHeight
        const screenHeightPercentage = window.innerHeight * 0.1 // 屏幕高度偏移量
        //需要随着滑动更改背景在这也加上上面注释代码
        if (pos >= min && pos <= max + screenHeightPercentage) {
          const items = this.timelineElement.querySelectorAll('.item')
          items.forEach((item, indexItem) => {
            items[indexItem].classList.remove('item--active')
          })
          items[index].classList.add('item--active')
        }
      })
    }
  }
}
</script>

<style scoped>
        .shell {
            width: 100%;
            position: relative;
            padding: 80px 0;
            background-attachment: fixed;
            background-size: cover;
            background-image: url("@/assets/image/experience_background.jpg");
        }

        .shell:before {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background: rgba(99, 99, 99, 0.8);
            content: "";
        }

        .header {
            width: 100%;
            text-align: center;
            margin-bottom: 80px;
            position: relative;
        }

        .title {
            color: #fff;
            font-size: 46px;
            font-weight: normal;
            margin: 0;
        }

        .timeline {
            display: flex;
            margin: 0 auto;
            flex-wrap: wrap;
            flex-direction: column;
            max-width: 700px;
            position: relative;
        }

        .content-title {
            font-weight: normal;
            font-size: 66px;
            margin: -10px 0 0 0;
            transition: 0.4s;
            padding: 0 10px;
            box-sizing: border-box;
            color: #fff;
        }

        .content-desc {
            margin: 0;
            font-size: 15px;
            box-sizing: border-box;
            color: rgba(255, 255, 255, 0.7);
            line-height: 25px;
        }

        .timeline:before {
            position: absolute;
            left: 50%;
            width: 2px;
            height: 100%;
            margin-left: -1px;
            content: "";
            background: rgba(255, 255, 255, 0.07);
        }

        .item {
            padding: 40px 0;
            opacity: 0.3;
            filter: blur(2px);
            transition: 0.5s;
            box-sizing: border-box;
            width: calc(50% - 40px);
            display: flex;
            position: relative;
            transform: translateY(-80px);
        }

        .item:before {
            /* 设置在伪元素before中的内容  */
            content: attr(data-text);
            letter-spacing: 3px;
            width: 100%;
            position: absolute;
            color: rgba(255, 255, 255, 0.5);
            font-size: 13px;
            border-left: 2px solid rgba(255, 255, 255, 0.5);
            top: 70%;
            margin-top: -5px;
            padding-left: 15px;
            opacity: 0;
            right: calc(-100% - 56px);
            font: 900 20px '';
            letter-spacing: 5px;
        }

        .item:nth-child(even) {
            align-self: flex-end;
        }

        .item:nth-child(even):before {
            right: auto;
            text-align: right;
            left: calc(-100% - 56px);
            padding-left: 0;
            border-left: none;
            border-right: 2px solid rgba(255, 255, 255, 0.5);
            padding-right: 15px;
        }

        .item--active {
            opacity: 1;
            transform: translateY(0);
            filter: blur(0px);
        }

        .item--active:before {
            top: 50%;
            transition: 0.3s all 0.2s;
            opacity: 1;
        }

        .item--active .content-title {
            margin: -50px 0 20px 0;
        }

        .img {
            max-width: 100%;
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.4);
        }

        .subtitle {
            color: rgba(255, 255, 255, 0.5);
            font-size: 16px;
            letter-spacing: 5px;
            margin: 10px 0 0 0;
            font-weight: normal;
        }

        .footer {
            padding: 95px 0;
            text-align: center;
        }

        .footer a {
            color: #999;
            display: inline-block;
        }

        @media only screen and (max-width: 767px) {
            .item {
                align-self: baseline !important;
                width: 100%;
                padding: 0 30px 150px 80px;
            }

            .item:before {
                left: 10px !important;
                padding: 0 !important;
                top: 50px;
                text-align: center !important;
                width: 60px;
                border: none !important;
            }

            .item:last-child {
                padding-bottom: 40px;
            }
        }

        @media only screen and (max-width: 767px) {
            .timeline:before {
                left: 40px;
            }
        }
</style>
