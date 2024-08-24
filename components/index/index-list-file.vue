<template>
  <view class="gallery">
	<!-- 显示图片 -->
	<block v-for="(item,index) in displayedMedia" :key="index" class="image-container">
		<template v-if="item.type === 'image'">
			<image :src="item.src" @load="onImageLoad(index, $event)" :class="getImageClass(item)" mode="aspectFill"></image>
	  </template>
	  <template v-else-if="item.type === 'video'">
	    <view class="video-container" >
	      <video :src="item.src" @loadedmetadata="onVideoLoadedMetadata(index, $event)" :class="getImageClass(item)" controls></video>
	    </view>
	  </template>
	</block>
  </view>
</template>

<script>
export default {
	props:{
		media:Array		
	},
  data() {
    return {
	  images:[],
	  videos:[]
    }; 
  },
  computed: {
    displayedMedia() {
      this.images = this.media.filter(item => item.type === 'image');
      this.videos = this.media.filter(item => item.type === 'video');

      // 确保至少保留一个视频
      let combinedMedia = [];

      if (this.videos.length) {
        // 如果有视频，则确保视频始终位于列表的末尾
        combinedMedia = [...this.images.slice(0, 2), this.videos[0]]; // 只保留前两个图片加上视频
      } else {
        combinedMedia = this.images.slice(0, 3); // 如果没有视频，则只保留前三张图片
      }

      // 最多只显示三个媒体文件
      return combinedMedia.slice(0, 3);
    }
  },
  methods: {
	  onImageLoad(index,e) {
		  console.log(e)
	    const { width, height } = e.detail;
		console.log(width)
	    this.media[index] = { ...this.media[index], width, height };
		console.log(index)
	  },
	  onVideoLoadedMetadata(index,e){
		  console.log(e)
		const { width, height } = e.detail;
		console.log(width)
		this.media[index] = { ...this.media[index], width, height };
		console.log(index)
		// 更新视频容器的类名
/* 		  const aspectRatio = width / height;
		  if (aspectRatio > 1) {
			this.$set(this.displayedMedia[index], 'class', 'video-4x3');
		  } else {
			this.$set(this.displayedMedia[index], 'class', 'video-3x4');
		  } */
	  },
    getImageClass(item) {
      if (this.images.length + this.videos.length> 1) {
		  if(item.type === "video"){
		  	return 'video-1x1';
		  }else{
		  	return 'image-1x1';
		  }   
      } else {
        const aspectRatio = item.width / item.height;
		console.log(aspectRatio)
        if (aspectRatio > 1) {
			if(item.type === "video"){
				return 'video-4x3';
			}else{
				return 'image-4x3';
			}     
        } else {
			if(item.type === "video"){
				return 'video-3x4';
			}else{
				return 'image-3x4';
			} 
        }
      }
    }
  }
};
</script>

<style scoped>
.gallery {
  display: flex;
  flex-wrap: nowrap;
  justify-content: left;
	padding-left: 8rpx;
 /* height: 220rpx;
  width: 690rpx; */
/*  height: 220rpx; */
  width: 690rpx;
}

.image-1x1 {
 width: 220rpx;
 height: 220rpx;
 &:last-child{
	 border-radius: 0 15rpx 15rpx 0;
	 };
 &:first-child{
	 border-radius: 15rpx 0 0 15rpx;
	 };
	padding: 0 4rpx;
}

.image-4x3 {
  width: 440rpx;
  height: 330rpx;
  border-radius: 15rpx;
}

.image-3x4 {
  width: 330rpx;
  height: 440rpx;
  border-radius: 15rpx;
}
.image-container{
	width: 100%;
	height: 100%;
}
/* .gallery image {
	width: 100%;
	height: 100%;
}
 */
.video-1x1{
	 width: 220rpx;
	 height: 220rpx;
	 &:last-child{border-radius: 0 15rpx 15rpx 0;};
}
.video-4x3{
  width: 440rpx;
  height: 330rpx;
  
}

.video-3x4{
  width: auto;
  height: 440rpx
}
</style>
