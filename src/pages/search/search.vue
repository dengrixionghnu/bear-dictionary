<template>
<view class="container">
  <image class="logo" src="../../static/search-logo.png"></image>
  <text class="name">小熊词典</text>
  <view class="input-container">
    <input class="input-search" v-model="search" @focus="resetMessage" placeholder="请输入..." type="text"/>
  </view>
  <view class="input-container">
  <text v-if="searchMessage" class="word-definition">{{searchMessage}}</text>
  </view>
  <view class="button-search" @click="doSearch">
    <text class="text-search" @click="doSearch">查询</text>
  </view>
</view>
    
</template>

<script>
 import wordRepository from '../../data/word-repository'

export default {
      data() {
        return {
          search:"",
          searchMessage:""
        }
      },
      onLoad() {
        this.search = "";
        this.searchMessage="";
      },
      methods: {
        doSearch: function (){
            console.log("jdjfadf"+this.search)
            if(!this.search){
                return
            }
            var wordList = wordRepository.getWordList();
            var index
            for (let i = 0; i < wordList.length; i++) {
                var context = wordList[i].content;
                if(context.startsWith(this.search)){
                    index = i;
                    break;
                }
            
            }
            if(index){
                uni.navigateTo({
                    url: '/pages/search/detail/detail?id=' + index,
                    success: function() { 
                        console.log('跳转成功');
                    },
                    fail: function(err) {
                        console.log('跳转失败', err);
                    }
                    });
            }else{
                this.searchMessage="没有找到";
            }

          } ,
          resetMessage:function(){
            this.searchMessage="";
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

.logo {
  width: 224rpx;
  height: 224rpx;
  margin-top: 118rpx;
}

.name {
  margin-top: 58rpx;
  font-family: Songti SC;
  font-size: 32rpx;
  color: #585858;
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

.button-search {
  margin: 10rpx;
  display: flex;
  flex-direction: column;
  width: 360rpx;
  height: 70rpx;
  border: 2rpx solid #353535;
  border-radius: 10rpx;
  align-items: center;
}

.text-search {
  margin: auto;
  font-family: Songti TC;
  font-size: 32rpx;
  color: #585858;
}

.word-definition {
    margin-top: 194rpx;
    font-family: Yuanti TC;
    font-size: 30rpx;
    color: #585858;
  }

</style>