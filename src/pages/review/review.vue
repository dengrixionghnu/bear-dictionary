<template>
    <view class="container">
        <text class="word-content">{{word.content}}</text>
        <view class="pron-container">
        <image class="pron-icon" src="../../static/pron-icon.png"></image>
        <text class="word-pron">/{{word.pron}}/</text>
        </view>
        <text class="word-definition">{{word.definition}}</text>

        <text v-if="message" class="word-definition">{{message}}</text>

        <view class="button-search" @click="remove">
            <text class="text-search" @click="remove">移除</text>
        </view>

        <view class="button-search" @click="next">
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
          pron: "",
          definition: ""},
          index:0,
          message:""
          
      }
    },
    onLoad() {
        this.index = 0;
        var id = wordRepository.gerReviewList()[this.index];
        var findword=wordRepository.getWordList()[id];
        this.word={...findword}
    
    },
    methods: {
        remove:function(){
            var id = wordRepository.gerReviewList()[this.index];
            const newArray =  wordRepository.gerReviewList().filter((element) => element !== id);
            wordRepository.gerReviewList().splice(0, wordRepository.gerReviewList().length);
            wordRepository.gerReviewList().push(...newArray)
            if(this.index>=wordRepository.gerReviewList().length){
                this.index = wordRepository.gerReviewList().length-1
            }
            this.message="删除成功"
            var id = wordRepository.gerReviewList()[this.index];
            var findword=wordRepository.getWordList()[id];
            this.word={...findword}
        
        },
        next:function(){
            this.message=""
            if(this.index<wordRepository.gerReviewList().length){
                this.index = this.index +1;
            }else{
                this.index = 0;
            }
          
            var id = wordRepository.gerReviewList()[this.index];
            var findword=wordRepository.getWordList()[id];
            this.word={...findword}
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
