<template>
 <view class="out">
     <view class="container">
       <view class="input-container">
         <view class="input-item">
           <text class="input-label">帖子标题：</text>
           <input v-model="post.title" class="input-field-title" placeholder="请输入帖子标题" />
         </view>
         <view class="input-item">
           <text class="input-label">帖子内容：</text>
           <textarea v-model="post.content" class="input-field-content" placeholder="请输入帖子内容"></textarea>
         </view>
         <view class="input-item">
           <text class="input-label">选择图片：</text>
           <button @click="chooseImage" class="upload-button">上传图片</button>
         </view>
         <view class="image-container">
           <view v-for="(image, index) in images" :key="index" class="image-item">
             <image :src="image" mode="aspectFit" class="image-src"></image>
           </view>
         </view>
       </view>
       <view class="button-container">
         <button @click="publishPost" class="publish-button">发布帖子</button>
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
<style scoped>
.out {
  padding: 20px;
  background-color: #f9f9f9;
  min-height: 100vh;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.input-container {
  display: flex;
  flex-direction: column;
}

.input-item {
  margin-bottom: 20px;
}

.input-label {
  font-size: 16px;
  color: #333;
  margin-bottom: 8px;
  display: block;
}

.input-field-title,
.input-field-content {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.input-field-content {
  min-height: 100px;
  resize: vertical;
}

.upload-button {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 14px;
  color: #fff;
  background-color: royalblue;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  text-align: center;
}

.upload-button:hover {
  background-color: #4169e1;
}

.image-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.image-item {
  flex: 1 1 calc(33.333% - 10px);
  box-sizing: border-box;
}

.image-src {
  width: 100%;
  height: auto;
  border-radius: 4px;
}

.button-container {
  margin: 20px 0;
  text-align: center;
}

.publish-button {
  display: inline-block;
  padding: 10px 20px;
  font-size: 14px;
  color: #fff;
  background-color: royalblue;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.publish-button:hover {
  background-color: #4169e1;
}
</style>