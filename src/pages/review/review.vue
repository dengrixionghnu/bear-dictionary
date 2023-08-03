<template>
    <view class="container">
        <image class="logo" src="../../static/search-logo.png"></image>
        <text class="name">浣熊词典</text>

        <text v-if = "hasReview" class="word-content">{{word.content}}</text>
        <text v-if = "hasReview" class="word-definition">{{word.definition}}</text>
        <text v-if = "!hasReview" class="word-definition">您还没有添加，可以标记一些，方便回顾哦</text>
        <view v-if = "hasReview" class="button-search" @click="remove">
            <text class="text-search" @click="remove">移除</text>
        </view>

        <view v-if = "hasReview" class="button-search" @click="next">
            <text class="text-search" @click="next">下一个</text>
        </view>

    </view>
</template>

<script>
import wordRepository from '../../data/word-repository'
  
  export default {
    data() {
      return {
        word:{
          content: "",
          definition: ""
        },
          index:0,
          message:"",
          hasReview:false
          
      }
    },
    onLoad() {
      this.initdata();
    
    },
    onShow() {
      this.initdata();

    },
    methods: {
        remove:function(){
            var id = wordRepository.gerReviewList()[this.index];
            wordRepository.removeReview(id);
            if(this.index>=wordRepository.gerReviewList().length){
                this.index = 0
            }
            this.getWord(); 
            uni.showToast({
              title:"移除成功",
              duration:500,
              icon:'success',
              mask:true
            })
        
        },
        next:function(){
            this.index = this.index +1;
            if(this.index>=wordRepository.gerReviewList().length){
              this.index = 0;
              uni.showToast({
              title:"没有了",
              duration:500,
              icon:'success',
              mask:true
            })
            }
            var id = wordRepository.gerReviewList()[this.index];
            this.getWord(id);
          },
          getWord: function() {
                this.hasReview = wordRepository.gerReviewList().length>0;
                if(!this.hasReview){
                  return;
                }
                var id = wordRepository.gerReviewList()[this.index];
                wordRepository.getWordByIndex(id, (word) => {
                  this.word = { ...word };
                });
          },
          initdata:function(){
            this.index = 0;
            this.hasReview = wordRepository.gerReviewList().length>0;
            if(!this.hasReview){
              return;
            }
            var id = wordRepository.gerReviewList()[this.index];
            this.getWord();
          }
  
    },
    mounted() {
        uni.addInterceptor('onHide', (options) => {
          // 在小程序退出之前执行保存数据的操作
          var array = wordRepository.gerReviewList();
          uni.setStorageSync('reviewList', array); 
          // 返回 true 继续执行 onHide 事件
          return true;
        });}
  }

</script>

<style>

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  justify-content: space-between;
  box-sizing: border-box;
}

.word-content {
  margin: 234rpx auto 18rpx;
  font-family: Chalkboard;
  font-size: 64rpx;
}

.pron-container {
  display: flex;
  flex-direction: row;
}

.logo {
  width: 100rpx;
  height: 100rpx;
}

.pron-icon {
  width: 48rpx;
  height: 48rpx;
}

.word-pron {
  margin-left: 9rpx;
  font-family: Chalkboard;
  font-size: 36rpx;
  color: #8F8F8F;
}

.word-definition {
  margin-top: 100rpx;
  margin-bottom: 100rpx;
  font-family: Yuanti TC;
  font-size: 30rpx;
  color: #585858;
}

.button-search {
  margin: 10ßrpx;
  display: flex;
  flex-direction: column;
  width: 360rpx;
  height: 70rpx;
  border: 2rpx solid #353535;
  border-radius: 10rpx;
  align-items: center;
}

</style>
