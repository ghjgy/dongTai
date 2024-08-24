<template>

		<scroll-view scroll-y class="list" @scrolltolower="loadMoreData"
		 scroll-with-animation="true">
			<template v-if="followList.length>0">
				<!--图文列表-->
				<block v-for="(item,index) in followList" :key="index">
					<indexList 
					 :item="item" 
					 :index="index"
					 @guanzhu="guanzhu(index)"
					 @caozuo="caozuo($event,index)">
					 </indexList>
				</block>
			</template>
			<view>
				<loadMore
				:loading="loading"
				:noMore="noMore"
				:loadError="loadError"
				 @loadMoreData="loadMoreData"
				></loadMore>
			</view>
		</scroll-view>
</template>

<script>
	import indexList from '../index/index-list.vue';
	import loadMore from '../common/load-more.vue';
	export default {
		name:"follows",
		props:{
			followList:Object			
		},
		components:{
			indexList,
			loadMore
		},
		data() {
			return {
				loadtext:"上拉加载更多👆",
				loading:false,
				noMore:false,
				loadError:false
			};
		},
		methods:{
			//关注
			guanzhu(index){			
				let o=this.followList.list[index];
				o.isguanzhu=true;
				uni.showToast({
					title:'关注成功',
					mask:true,
					icon:'none'
				})
			},
			//顶踩操作
			caozuo(type,index){
				let ok=this.followList.list[index];
				switch (type){
					case "ding":
					if(ok.biaoqing.index==1){return;}
					ok.biaoqing.dingnum++;					
					if(ok.biaoqing.index==2){
						ok.biaoqing.cainum--;
					}
					ok.biaoqing.index=1;
						break;
					case "cai":
					if(ok.biaoqing.index==2){return;}
					ok.biaoqing.cainum++;
					if(ok.biaoqing.index==1){
						ok.biaoqing.dingnum--;
					}
					ok.biaoqing.index=2;
						break;
			}
			},
			//上拉事件
			loadMoreData(){
				console.log("sadasdsad")
				if(this.noMore){return;}					
				this.loading=true;
				setTimeout(()=> {
					let obj={
						userimg:"../../static/userpic/10.jpg",
						username:"昵称",
						isguanzhu:false,
						title:"我是标题",
						type:"img",//img图片，video视频
						medias: [
						],
						biaoqing:{
							index:0,//0表示无操作，1表示顶了，2表示踩了
							dingnum:10,
							cainum:10
						}, 
						pinglunnum:10,
						sharenum:100
					};
					this.followList.push(obj);
					this.loading=false;
					 
					  
					//this.newlist[index1].loadtext="没有更多数据了/(ㄒoㄒ)/~~";
					 
				}, 1000);
			}
		}
	}
</script>

<style>

</style>