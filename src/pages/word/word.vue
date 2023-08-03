<template>
  <view class="container">
    <image class="logo" src="../../static/search-logo.png"></image>
    <text class="name">浣熊词典</text>

    <view class="input-container">
      <input class="input-search" v-model="search" v-on:blur="doSearch" placeholder="请输入..." type="text"/>
    </view>

    <view class="container">
      <text class="word-content">{{word.content}}</text>
      <text class="word-definition">{{word.definition}}</text>
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
            definition: "",
          },
          worldListMax:0,
          index:0,
          search:""
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
        this.index = Math.floor(Math.random() * this.worldListMax) +1;
        this.getWord();
      },
      methods: {
          next:function(){
            this.index = Math.floor(Math.random() * this.worldListMax) +1;
            this.getWord();
          },
          getWord: function() {
                wordRepository.getWordByIndex(this.index, (word) => {
                  this.word = { ...word };
                });
          },
          mark:function(){
            var array = wordRepository.gerReviewList();
            if(!wordRepository.gerReviewList().includes(this.index)){
              array.push(this.index);
            }
            uni.showToast({
              title:"标记成功",
              duration:500,
              icon:'success',
              mask:true
            })
          },
          doSearch: function (){
            if(!this.search){
                return
            }
            var wordList = wordRepository.getWordList();
            var index = -1;
            for (let i = 0; i < wordList.length; i++) {
                var context = wordList[i];
                console.log(context,this.search.toLowerCase)
                if(context.startsWith(this.search.toLowerCase())){
                    index = i;
                    break;
                }
            
            }
            if(index>=0){
               this.index = index;
               this.getWord();
               this.search = "";
            }else{
              uni.showToast({
              title:"没有找到",
              duration:500,
              icon:'failure',
              mask:true
            })
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

  .logo {
  width: 100rpx;
  height: 100rpx;
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

  .input-container {
  margin-top: 50rpx;
  width: 100%;
  height: 50rpx;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-search {
  margin: auto;
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