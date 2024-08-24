<template>
	<view class="index-list animated zoomInUp fast">
				 <view class="index-listone u-f-aj">
					 <view class="u-f-a">
						 <image :src="item.userimg" 
						 mode="aspectFill"
						  lazy-load></image>
						  <view class="user-info">
							  <view class="user-info-name">
							  	{{item.username}} 
							  </view>
						  	  <view class="user-info-time">
						  	  	 1分钟前
						  	  </view>
						  </view>
						 
					 </view>
					 <view class="u-f-a" v-show="!item.isguanzhu" @tap="oo">
						 <view class="icon iconfont icon-zengjia"></view>关注
					 </view>
				 </view>
				 <view class="index-listtwo" @tap="opendetail">
					    {{item.title}}
				 </view>
				 <view class="index-listtwo-half" @tap="opendetail">
				 					    {{item.contents}}
				 </view>
				 <view class="index-listthree " @tap="opendetail">
					 <!--图片&视频-->
					   <indexListFile
					   :medias="item.medias"
					   ></indexListFile>
				 </view>
				 <view class="index-listfour u-f-aj">
					 <view class="u-f-a">
						 <view class="u-f-a" hover-class="activegray" hover-stay-time="100" >
						 <view class="icon iconfont icon-zhuanfa"></view>
						 {{item.sharenum}}
						 </view>
						 <view class="u-f-a" hover-class="activegray" hover-stay-time="100" >
						 <view class="icon iconfont icon-pinglun1"></view>
						 {{item.pinglunnum}}
						 </view>
					 </view> 
					 <view class="u-f-a">
						 <view class="u-f-ajr" hover-class="animated rubberBand" 
						 hover-stay-time="1000" 
						 :class="{'activered':(item.biaoqing.index==1)}" 
						 @tap="caozuo('ding')">			    
						    <view class="icon iconfont icon-icon_xiaolian-mian"></view>
							<text>{{item.biaoqing.dingnum}}</text>	 							  				  
						 </view>
						 <view class="u-f-ajr" hover-class="animated rubberBand" 
						 hover-stay-time="1000" 
						 :class="{'activeblue':(item.biaoqing.index==2)}"
						  @tap="caozuo('cai')">
							 <view class="icon iconfont icon-kulian"></view>
							 <text>{{item.biaoqing.cainum}}</text>
						 </view>
					 </view>
				 </view>
				 <!-- <view class="container">
				   <view class="item" :style="{color: textColor}">
				     <view class="icon">
				       <image src="../../static/common/分享.png" mode="widthFix"></image>
				     </view>
				     <text class="text">2</text>
				   </view>
				   <view class="item" :style="{color: textColor}">
				     <view class="icon">
				       <image src="../../static/common/回复.png" mode="widthFix"></image>
				     </view>
				     <text class="text">431</text>
				   </view>
				   <view class="item" :style="{color: textColor}">
				     <view class="icon">
				       <image src="../../static/common/已赞-copy.png" mode="widthFix"></image>
					   
				     </view>
				     <text class="text">1163</text>
				   </view>
				 </view> -->
	</view>
</template>

<script>
	import indexListFile from '../index/index-list-file.vue';
	export default{
		props:{
			item:Object,
			index:Number			
		},
		components:{
			indexListFile
		},
		methods:{
			//关注
			oo(){						
			    this.$emit('guanzhu');
			},
			caozuo(type){
				 this.$emit('caozuo',type);								
			},
			opendetail(){
				const encodedItem = encodeURIComponent(JSON.stringify(this.item));
				uni.navigateTo({
					url:`../../pages/detail/detail?detailinfo=${encodedItem}`
				})
			},
/* 			getImageClass(im) {
			  if (this.item.medias.length > 1) {
				  console.log(this.item)
				return 'image-1x1';
			  } else {
				  console.log("cnbv")
				const aspectRatio = im.width / im.height;
				if (aspectRatio > 1) {
				  return 'image-4x3';
				} else {
				  return 'image-3x4';
				}
			  }
			} */
		}
	}
</script>

<style scoped>
.index-list{

	border-bottom: 12upx solid #F2F2F2;
	}
.index-listone>view:first-child{
	color: #989898;
}
.index-listone>view:first-child image{
	width: 85upx;
	height: 85upx;  
	border-radius: 100%;
	margin-right: 15upx;
}
.index-listone>view:last-child{
	background-color: #F4F4F4;
	color: #050505;
	padding: 0 15upx;
	font-weight: 600;
	border-radius: 5upx;
}
.index-listtwo{
	padding-top: 10upx;
	font-size: 35upx;
	font-weight: 600;
	letter-spacing: 1upx;
	color: #010101;
}
.index-listthree{
	
	margin-top: 15upx;
}
.index-listfour{
	padding: 10upx 0;
	margin-top: 20upx;
	color: #545454;	
}
.index-listfour>view:first-child>view{
	margin-right: 15upx;
	width: 100upx;
}
.index-listfour>view:last-child>view{
	width: 120upx;
}
.index-listfour>view:first-child>view>view{
	margin-right: 15upx;
}
.index-listfour>view:last-child>view>text{
	width: 50upx;
	text-align: right;
}
.index-listthree-play{
	position: absolute;
	font-size: 140upx;
	color: #FFFFFF;
}
.index-listthree-playinfo{
	position: absolute;
	bottom: 10upx;
	right: 10upx;
	background: rgba(51,51,51,0.72);
	color: #FFFFFF;
	font-size: 24upx;
	padding: 1upx 15upx;
	border-radius: 40upx;
}
.activered{
	color: #FF392C;
}
.activeblue{
	color: #2B80FF;
}
.activegray{
	color: #808080;
}
.user-info{
	height: 100%;
	
}
.user-info-name{
	font-size: 28rpx;
	padding-bottom: 0rpx;	
	font-weight: 600;
	color: #010101;
}
.user-info-time{
	padding-top: 0rpx;
	font-size: 22rpx;
	
}
.container {
	padding: 10upx 0;
	margin-top: 20upx;
	color: #545454;	
  display: flex;
  justify-content: space-between;
}

.item {
  display: flex;
  align-items: center;
}

/* .icon {
  width: 40rpx;
  height: 40rpx;
  image{
	  width: 40rpx;
	  height: 40rpx;
  }
} */

.text {
  font-size: 28rpx;
}
</style>
