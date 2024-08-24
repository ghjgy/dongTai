<template>
	<view class="topload">
		 <!-- 加载状态 -->
		 <view v-if="loading" class="loading">
			<uni-load-more status="loading"></uni-load-more>
		 </view>
		 <view v-else-if="noMore" class="loading">
			<uni-load-more status="noMore"></uni-load-more>
		 </view>
		 <view v-else-if="loadError" class="loading" @click="loadMoreData">加载失败，点击重试</view>
		 <view v-else-if="!loading &&!noMore" class="loading">
			<uni-load-more status="more" @click="loadMoreData"></uni-load-more>
		 </view>
		 <view class="safe-area-inset-bootom"></view>
	</view> 
</template>

<script>
	export default{
		props:{
			loading: Boolean,
			noMore: Boolean,
			loadError: Boolean,
		},
		methods:{
			loadMoreData(){
				this.$emit('loadMoreData');
				console.log("测试")
			}
		}
	}
</script>

<style scoped>
	.topload{
		padding: 15upx 0;
		text-align: center;
		font-size: 25upx;
		color: #010101;
	}
</style>
