<template>
	<view>
    <!-- 轮播图区域 -->
		    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
		      <swiper-item v-for="(item,i) in swiperList" :key="i">
		        <view class="swiper-item"> 
		            <!--这里是动态绑定了图片的地址，:src="..." -->
		          <image :src="item.url"></image>
		        </view>
		      </swiper-item>
		    </swiper>
        <!-- 功能导航区域 -->
        <view class="navi-list">
          <view v-for="(item,i) in naviList" :key="i" @click="navClickHandler(item)">
            <image :src="item.url" class="navi-item"></image>
          </view>
        </view>
        
        <!-- 分类导航图片 -->
       <view>
        <view class="cate-box">
          <view v-for="(item2,i2) in cateList" :key="i2">
            <image :src="item2.url" class="img_hw" @click="ToCate"></image>
          </view>
        </view>
        </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList: [],
        naviList: [],
        cateList: [],
			};
		},
    
    onLoad(){
      this.getswiperList();
      this.getnaviList();
      this.getcateList();
    },
    
    methods:{
      // 获取轮播图
      async getswiperList(){
        const {data:res} = await uni.$http.get('/pictures/1')
        if(!res.flag){
          uni.$showMsg();
        }else{
          this.swiperList=res.data
        }
        this.swiperList.forEach(item=>{
          item.url='http://localhost/files/download/'+item.fileid;
        })
      },
      // 获取功能导航栏
      async getnaviList(){
        const {data:res} = await uni.$http.get('/pictures/2')
        if(!res.flag){
          uni.$showMsg();
        }else{
          this.naviList=res.data
        }
        this.naviList.forEach(item=>{
          item.url='http://localhost/files/download/'+item.fileid;
        })
      },
      // 点击导航栏跳转
      navClickHandler(item){
        // console.log(item);
        if(item.fileid === 24){
          uni.navigateTo({
            url:'/subpackge/calender/calender'
          })
        }else{
          uni.navigateTo({
            url:'/subpackge/calculator/caculator'
          })
        }
      },
      // 部分分类展示和点击跳转
      async getcateList(){
        const {data:res} = await uni.$http.get('/pictures/4')
        console.log(res);
        if(!res.flag){
          uni.$showMsg();
        }else{
          this.cateList=res.data
        }
        this.cateList.forEach(item=>{
          item.url='http://localhost/files/download/'+item.fileid;
        })
      },
      ToCate(){
        uni.switchTab({
          url:'/pages/cate/cate'
        })
      }
      
    }
    
    
	}
</script>

<style lang="scss">
  swiper {
    height: 330rpx;
    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }
  
  .navi-list{
    display: flex;
    justify-content: space-around;
    margin: 15px,0;
    padding-top: 8px;
    background-color: #F1F1F1;
 
  .navi-item{
    width: 130rpx;
    height: 130rpx;
    border: 2px solid #F0AD4E;
    border-radius: 8px;
  }
   }
  .cate-box{
    padding-top: 15px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .img_hw{
    height: 135px;
    width: 150px;
    border: 2px solid #AAFFFC;
    border-radius: 8px;
    margin-top: 5px;
    margin-left: 5px;
  }
</style>
