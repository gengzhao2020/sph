<template>
  <div class="swiper-container">
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="(slide, index) in skuImageList" :key="slide.id">
        <img 
          :src="slide.imgUrl" 
          :class="{ active: currentIndex === index }" 
          @click="changeCurrentIndex(index)" 
        />
      </div>
    </div>
    <div class="swiper-button-next"></div>
    <div class="swiper-button-prev"></div>
  </div>
</template>

<script>
  import Swiper from 'swiper'

  export default {
    name: "ImageList",
    props: ['skuImageList'],
    watch: {
      skuImageList() {
        this.$nextTick(() => {
          new Swiper(".swiper-container", {                     
            navigation: {  // 如果需要前进后退按钮
              nextEl: '.swiper-button-next',
              prevEl: '.swiper-button-prev',
            },
            slidesPerView: 3,
            slidesPerGroup: 1
          })
        })
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    methods: {
      changeCurrentIndex(index) {
        this.currentIndex = index
        this.$bus.$emit('getIndex', this.currentIndex)  // 通知 Zoom 组件，当前的索引值为几
      }
    }
  }
</script>

<style lang="less" scoped>
  .swiper-container {
    height: 56px;
    width: 412px;
    box-sizing: border-box;
    padding: 0 12px;

    .swiper-slide {
      width: 56px;
      height: 56px;

      img {
        width: 100%;
        height: 100%;
        border: 1px solid #ccc;
        padding: 2px;
        width: 50px;
        height: 50px;
        display: block;

        &.active {
          border: 2px solid #f60;
          padding: 1px;
        }
      }
    }

    .swiper-button-next {
      left: auto;
      right: 0;
    }

    .swiper-button-prev {
      left: 0;
      right: auto;
    }

    .swiper-button-next,
    .swiper-button-prev {
      box-sizing: border-box;
      width: 12px;
      height: 56px;
      background: rgb(235, 235, 235);
      border: 1px solid rgb(204, 204, 204);
      top: 0;
      margin-top: 0;
      &::after {
        font-size: 12px;
      }
    }
  }
</style>
