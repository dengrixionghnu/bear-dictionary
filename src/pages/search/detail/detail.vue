<template>
    <view class="container">
        <text class="word-content">{{word.content}}</text>
        <view class="pron-container">
        <image class="pron-icon" src="../../../static/pron-icon.png"></image>
        <text class="word-pron">/{{word.pron}}/</text>
        </view>
        <text class="word-definition">{{word.definition}}</text>

        <view class="button-search" @click="goback">
            <text class="text-search" @click="goback">返回</text>
        </view>
    </view>
</template>

<script>
import wordRepository from '../../../data/word-repository'
  
  export default {
    data() {
      return {
        word:{
          content: "",
          pron: "",
          definition: ""}
      }
    },
    onLoad(option) {
        var index = option.id;
        var findword = wordRepository.getWordList()[index];
        this.word={...findword}
    
    },
    methods: {
        goback:function(){
            uni.switchTab({
                    url: '/pages/search/search',
                    success: function() { 
                        console.log('跳转成功');
                    },
                    fail: function(err) {
                        console.log('跳转失败', err);
                    }
                    })

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

.button-search {
    margin: 226rpx;
  display: flex;
  flex-direction: column;
  width: 360rpx;
  height: 70rpx;
  border: 2rpx solid #353535;
  border-radius: 10rpx;
  align-items: center;
}

</style>
