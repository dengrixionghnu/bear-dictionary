<template>
  <view class="container">
      <text class="word-content">{{word.content}}</text>
      <view class="pron-container">
        <image class="pron-icon" src="../../static/pron-icon.png"></image>
        <text class="word-pron" @click="read">/{{word.pron}}/</text>
      </view>
      <view class="pron-container">
      <text v-show="showNot" class="word-definition">{{word.definition}}</text>
      <text v-if="message" class="word-definition">{{message}} </text>
      </view>


      <view class="button-next" @click="show(true)">
        <text class="word-next" @click="show(true)">查看</text>
      </view>
      <view class="button-next" @click="mark()">
        <text class="word-next" @click="mark()">标记</text>
      </view>
      <view class="button-next" @click="next">
        <text class="word-next" @click="next">下一个</text>
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
            definition: "",
            audioUrl: ""
          },
          worldListMax: null,
          vocListMax: null,
          showNot:false,
          index:-1,
          message:""
        }
      },
      onLoad() {
        this.worldListMax = wordRepository.getWordList().length
        this.word = {};
        var array = wordRepository.gerReviewList();
        var reviewList = uni.getStorageSync('reviewList', array); 
        if(!reviewList){
          array.push(...reviewList);
        }
        this.getWord()
      },
      methods: {
          read: function (){
            if(this.data.audioUrl){
                console.log("read not supported")
            }    
          },
          show: function(result){
            this.message = ""
            this.showNot = result;
          },
          next:function(){
            this.message = ""
            this.show(false)
            this.getWord();
          },
          getWord:function(){
            var index = Math.floor(Math.random() * this.worldListMax) +1
            this.index=index;
            var word = wordRepository.getWordList()[index] 
            this.word ={...word};
          },
          mark:function(){
            this.message = "标记成功"
            this.showNot = false;
            var array = wordRepository.gerReviewList();
            console.log("add review"+this.index)
            if(!wordRepository.gerReviewList().includes(this.index)){
              array.push(this.index);
            }
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
    margin: 200rpx auto 18rpx;
    font-family: Chalkboard;
    font-size: 64rpx;
    word-wrap: break-word;
  }
  
  .pron-container {
    display: flex;
    flex-direction: row;
    margin-bottom: 10px;
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
  
  .button-miss {
    display: flex;
    flex-direction: column;
    width: 360rpx;
    height: 70rpx;
    border: 2rpx solid #353535;
    border-radius: 10rpx;
    align-items: center;
    margin-top: 157rpx;
    margin-bottom: 37rpx;
  }
  
  .word-miss {
    margin: auto;
    font-family: Songti TC;
    font-size: 32rpx;
    color: #585858;
  }
  
  .button-next {
    display: flex;
    margin:10px;
    flex-direction: column;
    width: 360rpx;
    height: 70rpx;
    border: 2rpx solid #353535;
    border-radius: 10rpx;
    align-items: center;
  }
  
  .word-next {
    margin: auto;
    font-family: Songti TC;
    font-size: 32rpx;
    color: #585858;
  }
  
  
  </style>