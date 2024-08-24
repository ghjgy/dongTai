<template>
	<view class="home">
		<swiperHead 
		:tablist="tablist"
		:tabindex="tabindex"
		:linewidth="linewidth"
		:scroll="scroll"
		@tabtap="tabtap"
		@watchscroll="watchscroll"
		></swiperHead>
		<view class="uni-tab-bar">
		 <swiper class="swiper-box" :style="{height:swheigth+'px'}" :current="tabindex" @change="watchchange"> 
				<swiper-item>
					<follows
					:followList="followList"
					></follows>
				</swiper-item>
		</swiper>
		<view class="safe-area-inset-bootom"></view>
	    </view>
	</view>
</template>

<script>
	import indexList from '../../components/index/index-list.vue';
	import follows from '../../components/follows/follows.vue';
	import swiperHead from '../../components/index/swiper-head.vue';
	var _self;
	export default {
		components:{
			indexList,
			swiperHead
		},
		data() {
			return {
				loadtext:"上拉加载更多👆",
				swheigth:0,
				tabindex:0,
				linewidth:38,
				scroll:0,
				scrollwidth:360,//这个值通过swiperHead计算得来的，可以进去看一下
				tablist:[
					{name:"关注",id:"tuijian"},
					{name:"最新",id:"yule"},
					{name:"热榜",id:"xiha"},
 					{name:"搜索",id:"meinv"},
/*					{name:"科技",id:"keji"},
					{name:"财富",id:"caifu"} */
				],
				followList:[
					{
						userimg:"../../static/userpic/10.jpg",
						username:"昵称",
						guanzhupd:true,
						title:"中共中央举行纪念邓小平同志诞辰120周年座谈会习近平发表重要讲话",
						contents:"新华社北京8月22日电 中共中央22日上午在人民大会堂举行座谈会，纪念邓小平同志诞辰120周年。中共中央总书记、国家主席、中央军委主席习近平发表重要讲话。他强调，邓小平同志是全党全军全国各族人民公认的享有崇高威望的卓越领导人，伟大的马克思主义者，伟大的无产阶级革命家、政治家、军事家、外交家，久经考验的共产主义战士",						
						type:"img",//img图片，video视频
						media: [
						{ type: 'image', src: '../../static/datapic/16.jpg', width: 1024, height: 2768 },
						{ type: 'image', src: '../../static/datapic/15.jpg', width: 1500, height: 500 },
						{ type: 'image', src: '../../static/datapic/11.jpg', width: 1500, height: 500 },
						{ type: 'video', src: '../../static/datapic/49.mp4' }
						// ... 更多媒体项
						],
						biaoqing:{
							index:1,//0表示无操作，1表示顶了，2表示踩了
							dingnum:10,
							cainum:10
						},
						pinglunnum:10,
						sharenum:100
					},
					{
						userimg:"../../static/userpic/10.jpg",
						username:"昵称",
						guanzhupd:true,
						title:"直击辽宁葫芦岛抗洪抢险一线",
						contents:"受持续强降雨影响，辽宁省葫芦岛市建昌县、绥中县部分乡镇出现险情，部分人员被困。8月20日，辽宁省消防救援总队调派8车、27人在葫芦岛市开展抢险救援，出动省机动支队66人、18车、14艇增援。截至目前，应急救援、人员转移等工作还在持续进行中。(尹柏寒)",
						type:"video",//img图片，video视频
						playnum:"20w",
						playtime:"2:40",
						media: [
						{ type: 'image', src: '../../static/datapic/16.jpg', width: 1024, height: 2768 },

						// ... 更多媒体项
						],
						biaoqing:{
							index:1,//0表示无操作，1表示顶了，2表示踩了
							dingnum:10,
							cainum:10
						},
						pinglunnum:10,
						sharenum:100
					}
				]
							
			}
		},
		onNavigationBarSearchInputClicked(){
			uni.navigateTo({
			    url: '../search/search'			   
			});
		},
		onNavigationBarButtonTap(e){
			if(e.index==1){
				uni.navigateTo({
				   url:'../fabu/fabu'	   
				});
			}
		},
/* 		onReachBottom(){
			this.loadMoreData()
		}, */
		methods:{
			watchscroll(e){			
			},
			tabtap(index){
				this.tabindex=index;
			},
			//关注
			guanzhu(index1){			
				let o=this.newlist[this.tabindex].list[index1];
				o.guanzhupd=true;
				uni.showToast({
					title:'关注成功',
					mask:true,
					icon:'none'
				})
			},
			//顶踩操作
			caozuo(type,index2){
				let ok=this.newlist[this.tabindex].list[index2];
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
			//监听导航的index
			watchchange(e){
				this.tabindex=e.detail.current;
			},
			//上拉事件
			loadMoreData(){
				console.log("sadasdsad")
				if(this.loadtext!="上拉加载更多👆"){return;}					
				this.loadtext="加载中o(*￣▽￣*)ブ";
				setTimeout(()=> {
					let obj={
						userimg:"../../static/userpic/10.jpg",
						username:"昵称",
						guanzhupd:false,
						title:"中共中央举行纪念邓小平同志诞辰120周年座谈会习近平发表重要讲话",
						contents:"新华社北京8月22日电 中共中央22日上午在人民大会堂举行座谈会，纪念邓小平同志诞辰120周年。中共中央总书记、国家主席、中央军委主席习近平发表重要讲话。他强调，邓小平同志是全党全军全国各族人民公认的享有崇高威望的卓越领导人，伟大的马克思主义者，伟大的无产阶级革命家、政治家、军事家、外交家，久经考验的共产主义战士，党的第二代中央领导集体的核心，中国社会主义改革开放和现代化建设的总设计师，中国特色社会主义道路的开创者，邓小平理论的主要创立者，为世界和平和发展作出重大贡献的伟大国际主义者。他对党、对人民、对国家、对民族、对世界作出了突出贡献，功勋彪炳史册、永励后人。",
						type:"img",//img图片，video视频
						titleimg:[
					
							"../../static/datapic/16.jpg"
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
					this.loadtext="上拉加载更多👆";
					 
					  
					//this.newlist[index1].loadtext="没有更多数据了/(ㄒoㄒ)/~~";
					 
				}, 1000);
			}				
		},
		watch:{
			tabindex(e){
				//console.log(e);
				//监听index来滑动导航栏
				if(e==0){
				this.scroll=this.scrollwidth*(0/100);
				}
				else if(e==1){
				this.scroll=this.scrollwidth*(5/100);
				}
				else if(e==2){
				this.scroll=this.scrollwidth*(10/100);
				}
				else if(e==3){
				this.scroll=this.scrollwidth*(10/100);
				}
				else if(e==4){
				this.scroll=this.scrollwidth*(15/100);
				}
				else{
				this.scroll=this.scrollwidth*(100/100);
				}							
			}
		},
		onLoad() {	
		uni.getSystemInfo({
		    success:res=> {
				console.log(res)
		        let height=res.windowHeight-uni.upx2px(100);
				this.swheigth=height;	
		    }
		});				  
		}
		
	}
</script>
<style scoped>
.home{
	overflow: hidden;
}
</style>



