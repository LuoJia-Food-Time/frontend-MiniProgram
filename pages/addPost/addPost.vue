<template>
  <view class="out">
    <view class="container">
      <view class="input-container">
        <view class="input-item">
          <text class="input-label">帖子标题：</text>
          <input  v-model="post.title" class="input-field-title" placeholder="请输入帖子标题" />
        </view>
        <view class="input-item">
          <text class="input-label">帖子内容：</text>
          <textarea v-model="post.content" class="input-field-content" placeholder="请输入帖子内容"></textarea>
        </view>
        <view class="input-item">
          <text class="input-label">选择图片：</text>
          <button @click="chooseImage" style="background-color: royalblue;margin-left: 20% ;margin-right: 20%;">上传图片</button>
          <!--          <image v-if="post.image" :src="post.image" class="post-image" mode="aspectFill" /> -->
        </view>
        <view class="image-container" >
          <view v-for="(image, index) in images" :key="index" class="image-item" >
            <image :src="image" mode="aspectFit" class="image-src"></image>
          </view>
        </view>
        <!-- <view class="input-item">
          <text class="input-label">选择分类：</text>
          <picker mode="selector" :range="classification" v-model="selectedCategoryIndex" @change="categoryChange">
            <view class="picker">当前选择：{{ classification[selectedCategoryIndex] }}</view>
          </picker>
        </view> -->
      </view>
      <view class="button-container" style="margin: 20%">
        <button @click="publishPost" style="background-color: royalblue;">发布帖子</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      images: [], // 存储已上传图片的数组
      post: {
        title: '',
        content: '',
        images: '', // 存储已上传图片的数组
        classification: ''
      },
      classification: ['分类1', '分类2', '分类3', '分类4'],
      selectedCategoryIndex: 0
    };
  },
  methods: {
    //选择图片进行上传
    chooseImage() {
      uni.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success: (res) => {
          const tempFilePaths = res.tempFilePaths;
          this.images.push(tempFilePaths[0]);
        }
      });
    },
    //选择分类
    categoryChange(e) {
      this.selectedCategoryIndex = e.detail.value;
    },
    //发布帖子
    async publishPost() {
      this.post.classification = this.classification[this.selectedCategoryIndex];
      this.post.images = this.images.join(',');
      console.log(this.post.categorie);
      if (!this.post.title || !this.post.content || !this.post.images || !this.post.classification) {
        uni.showToast({
          title: '请填写完整信息',
          icon: 'none'
        });
        return;
      }
      // const res = await this.$myRequest({
      //   method: 'post',
      //   url: '/fatie',
      //   data: {
      //     title: this.post.title,
      //     content: this.post.content,
      //     images: this.post.images,
      //     classification: this.post.classification,
      //     userId: this.$store.state.user.id,
      //     communityId: this.$store.state.communityId
      //   }
      // });
	  else{
		  uni.showToast({
		    title: '发布成功',
		    icon: 'success'
		  });
		  uni.navigateTo({
		  					url:'/pages/community/community'
		  				});
	  }
    
    }
  }
};
</script>
<style>
<style scoped>
.out{
	margin: 10px;
}
.container {
  padding: 20px; /* 添加上下左右的内边距 */
  margin: 10px;
}

.input-container {
  /* 可以在需要的地方添加更多样式 */
}

.input-item {
  margin-bottom: 20px; /* 每个输入项之间添加一些底部边距 */
}

.input-label {
  margin-right: 10px; /* 输入项标签与输入框之间添加一些右侧边距 */
  font-size: 20px;
}

.input-field-title{
	height:calc(100% - 76px);
	width: calc(90%);
  border: 2px solid #ccc; /* 添加灰色实线边框 */
  padding: 8px; /* 添加内边距，使内容不贴着边框 */
}
.input-field-content {
	width: calc(90%);
  border: 2px solid #ccc; /* 添加灰色实线边框 */
  padding: 8px; /* 添加内边距，使内容不贴着边框 */
}


.button-container {
  text-align: center; /* 按钮居中 */
}

button {
  padding: 10px 20px; /* 按钮的内边距 */
}
</style>


</style>