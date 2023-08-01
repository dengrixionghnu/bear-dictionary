<template>
  <view class="container">
      <text class="word-content">{{word.content}}</text>
      <view class="pron-container">
        <image class="pron-icon" src="/static/pron-icon.png"></image>
        <text class="word-pron" @click="read">/{{pron}}/</text>
      </view>
      <text v-if="showNot" class="word-definition">{{word.definition}}</text>
      <view class="button-miss" @click="show(true)">
        <text class="word-miss" @click="show(true)">不认识</text>
      </view>
      <view class="button-next" @click="next">
        <text class="word-next" @click="next">下一个</text>
      </view>
  </view>
  
  </template>
  
  
  
  <script>
  import wordList from '../../data/word-list'
  
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
          showNot:false
        }
      },
      onLoad() {
        this.worldListMax = 1000
        this.word = {};
        this.getWord()
      },
      methods: {
          read: function (){
            if(this.data.audioUrl){
                console.log("read not supported")
            }    
          },
          show: function(result){
            this.showNot = result;
          },
          next:function(){
            this.show(false)
            this.getWord();
          },
          getWord:function(){
            var index = Math.floor(Math.random() * this.worldListMax) +1
            var word = wordList.wordList[index] 
            this.word ={...word};
          }
      }
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
    margin-top: 194rpx;
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