<template>
	<view class="common-list u-f-d">

		 <view class="common-list-two">
			 <view class="index-listone u-f-aj">
			 					 <view class="u-f-a">
			 						 <image :src="item.userimg" 
			 						 mode="aspectFill"
			 						  lazy-load></image>
			 						  <view class="user-info">
			 							  <view class="user-info-name">
			 							  	<view>{{item.username}}</view>
			 							  	 
			 							  	<tagSex :sex="item.sex" :userage="item.userage"></tagSex>
			 							  </view>
			 						  	  <view class="user-info-time">
			 						  	  	 <view class="common-list-two-time">26天前</view> 
			 						  	  </view>
			 						  </view>
			 						 
			 					 </view>
			 					 <view class="u-f-a" v-show="!item.isguanzhu"  @tap="isguan(index)">
			 						 <view class="icon iconfont icon-zengjia"></view>关注
			 					 </view>
			 </view>
			 <view class="index-listtwo" >
			 					    {{item.title}}
			 </view>
			 <view class="index-listtwo-half" >
			 					    {{item.contents}}
			 </view>
			 <view class="u-f-l">
			
				 <!-- 显示图片 -->
				 <block v-for="(media,index) in item.medias" :key="index">
				 	<template v-if="media.type === 'image'">
				 		<image :src="media.src" mode="widthFix" class="responsive-img" 
					  @tap="clickimg(index)" ></image>
				   </template>
				   <template v-else-if="media.type === 'video'">
				     <view class="" >
				       <video :src="media.src" class="responsive-video" controls></video>
				     </view>
				   </template>
				 </block>
				 <!-- 左图右文 -->
				 <view class="u-f-aj list-news" v-if="item.sharea">
					 <image :src="item.sharea.shareimg" mode="widthFix"></image>
					 <view>{{item.sharea.sharetitle}}</view>
				 </view>
			 </view>
			 <view class="u-f-aj">
				 <view>
					{{item.positiona}}
				 </view>
				 <view class="u-f-d">
					 <view class="icon iconfont icon-zhuanfa">{{item.sharenum}}</view>
					 <view class="icon iconfont icon-pinglun1">{{item.pinglunnum}}</view>
					 <view class="icon iconfont icon-ccdbaa">{{item.zannum}}</view>
				 </view>
			 </view>
		 </view>
	</view>
</template>

<script>
	import tagSex from "../common/tag-sex.vue";
	export default {
		components:{
			tagSex
		},
		props:{
			item:Object,
			index:Number
		},
		methods:{
			isguan(index){
				this.$emit('isgz',index);
			},
			clickimg(index){
				 // 预览图片
				       uni.previewImage({
						current:index,
						urls: this.item.medias,
						indicator:'default', 
						longPressActions: {
							itemList: ['发送给朋友', '保存图片', '收藏'],
							success: function(data) {
								console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
							},
							fail: function(err) {
								console.log(err.errMsg);
							}
				            }
				       });
			}
		}
	}
</script>

<style scoped>
    @import "../../common/list.css";
	.common-list-two{
		border-bottom: none;
	}
	.common-list{
		border-bottom: 1upx solid #EEEEEE;
	}
	.common-list-two-time{
		color: #CDCDCD!important;
		font-size: 25upx!important;
		padding: 0!important;
		background: #FFFFFF!important;
	}
	.common-list-two>view:nth-child(1)>view:nth-child(1)>view:nth-child(1)>view:nth-child(1){
	    color: #9B9B9B;
		font-size: 32upx;
	}
	.common-list-two>view:nth-child(1)>view:nth-child(1)>view:nth-child(2){
		margin-right: 20upx;
		color: #050505;
		padding: 0 15upx;
		font-weight: 600;
		border-radius: 5upx;
		font-size: 28upx;
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
	.user-info{
		height: 100%;
		
	}
	.user-info-name{
		 display: flex;
		 align-items: center;
		font-size: 28rpx;
		padding-bottom: 0rpx;	
		font-weight: 600;
		color: #010101;
	}
	.user-info-time{
		padding-top: 0rpx;
		font-size: 22rpx;
		
	}
	.responsive-img{
		margin-bottom: 10rpx;
		width: 690rpx;
		border-radius: 15rpx;
	}
	.responsive-video {
	  width: 690rpx; /* 设置宽度 */
	  position: relative; /* 设置相对定位 */
	  padding-bottom: 56.25%; /* 视频原始比例为 16:9，计算方式为 (9 / 16 * 100%) */
	  height: 0; /* 高度设为 0，利用 padding-bottom 来保持宽高比 */
	  overflow: hidden; /* 隐藏溢出的内容 */
	  border-radius: 15rpx;
	}
	
	.responsive-video::before,
	.responsive-video::after {
	  content: ""; /* 使用伪元素来创建一个容器 */
	  position: absolute; /* 绝对定位 */
	  top: 0;
	  left: 0;
	  right: 0;
	  bottom: 0;
	}
	
	.responsive-video video {
	  position: absolute; /* 绝对定位 */
	  top: 0;
	  left: 0;
	  width: 100%; /* 视频宽度设置为 100% */
	  height: 100%; /* 视频高度设置为 100% */
	  object-fit: cover; /* 保持宽高比，填充容器 */
	}
</style>
