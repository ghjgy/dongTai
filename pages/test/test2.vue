<template>
  <view class="gallery">
    <block v-for="(item, index) in displayedMedia" :key="index">
      <template v-if="item.type === 'image'">
        <image :src="item.src" @load="onImageLoad" :class="getImageClass(item)" mode="aspectFill"></image>
      </template>
      <template v-else-if="item.type === 'video'">
        <view class="video-container">
          <video :src="item.src" controls></video>
        </view>
      </template>
    </block>
  </view>
</template>

<script>
export default {
  data() {
    return {
      media: [
        { type: 'image', src: 'https://example.com/image1.jpg' },
        { type: 'image', src: 'https://example.com/image2.jpg' },
        { type: 'image', src: 'https://example.com/image3.jpg' },
        { type: 'video', src: 'https://example.com/video.mp4' }
        // ... 更多媒体项
      ]
    };
  },
  computed: {
    displayedMedia() {
      const images = this.media.filter(item => item.type === 'image');
      const videos = this.media.filter(item => item.type === 'video');

      // 确保至少保留一个视频
      let combinedMedia = [];

      if (videos.length) {
        // 如果有视频，则确保视频始终位于列表的末尾
        combinedMedia = [...images.slice(0, 2), videos[0]]; // 只保留前两个图片加上视频
      } else {
        combinedMedia = images.slice(0, 3); // 如果没有视频，则只保留前三张图片
      }

      // 最多只显示三个媒体文件
      return combinedMedia.slice(0, 3);
    }
  },
  methods: {
    onImageLoad(e) {
      const { width, height } = e.detail;
      const index = e.currentTarget.dataset.index; // 获取当前图片在数组中的索引
      this.media[index] = { ...this.media[index], width, height };
    },
    getImageClass(item) {
      if (this.displayedMedia.filter(m => m.type === 'image').length > 1) {
        return 'image-1x1';
      } else {
        const aspectRatio = item.width / item.height;
        if (aspectRatio > 1) {
          return 'image-4x3';
        } else {
          return 'image-3x4';
        }
      }
    }
  }
};
</script>

<style scoped>
.gallery {
  display: flex;
  flex-direction: column; /* 改为垂直排列 */
}

.image-1x1 {
  width: 100%; /* 改为100%宽度 */
  height: 0;
  padding-top: 100%; /* 保持1:1的比例 */
  position: relative;
}

.image-4x3 {
  width: 100%; /* 改为100%宽度 */
  height: 0;
  padding-top: 75%; /* 保持4:3的比例 */
  position: relative;
}

.image-3x4 {
  width: 100%; /* 改为100%宽度 */
  height: 0;
  padding-top: 133.33%; /* 保持3:4的比例 */
  position: relative;
}

.gallery image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.video-container {
  width: 100%;
}

.video-container video {
  width: 100%;
  height: auto;
}
</style>