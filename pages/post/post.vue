<template>
  <view style="height: 100%">
    <!-- moment信息 -->
    <view style="margin: 2%">
      <view class="moment-container">
        <!-- 头像 -->
        <image class="moment-avatar" :src="moment.userAvatarUrl" mode="aspectFill"></image>

        <!-- 用户名，发布时间 -->
        <view class="moment-user-time-wrapper">
          <view class="moment-user-name-wrapper">
            <view class="moment-user-name">
              {{ moment.userName }}
            </view>
            <view class="moment-notification-type">
              {{ notificationTypeDesc }}
            </view>
          </view>
          <view class="moment-create-time" :class="fromNotification ? 'moment-create-time-right' : ''">
            {{ moment.createTime | chineseTimeFormat }}
          </view>
        </view>
      </view>
      <!-- 动态内容 -->
      <view class="moment-text-content">
        {{ moment.content }}
      </view>

      <!-- 动态图片 -->
      <image class="moment-img" :src="moment.imgUrl" mode="aspectFill"></image>
    </view>
    <view class="tab-nav-main-wrapper">
      <view class="tab-nav-main">
        <view class="tab-nav-list" ref="tabNavList">
          <view class="tab-nav-item" :class="curTabIndex == 0 ? 'tab-nav-item-active' : ''" :data-index="0" @click="clikeTabNav">
            评论
            <text class="tab-nav-item-num">3</text>
          </view>
          <view class="tab-nav-item" :class="curTabIndex == 1 ? 'tab-nav-item-active' : ''" :data-index="1" @click="clikeTabNav">
            转发
            <text class="tab-nav-item-num">1</text>
          </view>
          <view class="tab-nav-item" :class="curTabIndex == 2 ? 'tab-nav-item-active' : ''" :data-index="2" @click="clikeTabNav">
            赞
            <text class="tab-nav-item-num">9</text>
          </view>
        </view>
      </view>
    </view>
    <view class="line"></view>
   
        <view class="comment-list">
          <view class="comment-title">
            {{ commentTitle }}
          </view>
          <view v-for="(c, index) in commentInfo.commentList" :key="index" class="comment-root" @click="goToReplyComment(c)">
            <view class="comment-wrapper">
              <!-- 头像 -->
              <image class="comment-avatar" mode="aspectFill" :src="c.userAvatarUrl" @click.stop="clickUser('COMMENT', c)"></image>
              <view class="comment-info">
                <!-- 用户名 -->
                <view class="comment-user-name">
                  {{ c.userName }}
                </view>
                <!-- 发布时间 -->
                <view class="comment-create-time">
                  {{ c.createTime }}
                </view>
                <!-- 评论内容 -->
                <view class="comment-content">
                  <u-parse :content="c.content"></u-parse>
                </view>
              </view>
            </view>
            <view class="comment-occupy-position-wrapper">
              <view class="comment-occupy-position"></view>
            </view>
          </view>
        </view>
     
<view class="comment-publish-area">
		<view class="comment-publish-input-wrapper">
			<textarea @confirm="publishComment" @blur="textAreaBlur" v-model="newComment.content" class="comment-publish-input"
			 auto-height="true" required :placeholder="parentComment!==null ? '回复'+parentComment.userName+'：' : '写评论...' ">
			</textarea>
		</view>
		<view id="commentPublishButton" class="comment-publish-button" :class="newComment.content ? '' : 'comment-publish-button-gray'"
		 @click="publishComment">
			发送
		</view>
	</view>

  </view>
</template>

<script>
import uParse from "@/components/gaoyia-parse/parse.vue";
import httpUtils from '../../common/util/httpUtils.js';
import publishComment from '../../components/publishComment/publishComment.vue';
import momentDetailRepost from '../../components/momentDetailRepost/momentDetailRepost.vue';
import momentDetailLike from '../../components/momentDetailLike/momentDetailLike.vue';

export default {
  components: {
    uParse,
    publishComment,
    momentDetailRepost,
    momentDetailLike
  },
 filters: {
    chineseTimeFormat(value) {
      // 假设你有一个函数可以格式化时间
      if (!value) return '';
      const date = new Date(value);
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const day = date.getDate().toString().padStart(2, '0');
      const hours = date.getHours().toString().padStart(2, '0');
      const minutes = date.getMinutes().toString().padStart(2, '0');
      return `${year}/${month}/${day} ${hours}:${minutes}`;
    }
  },
  data() {
	  
    return {
		newComment: {
			id: 1,
			userAvatarUrl: "/static/image/robot.png",
			userName: "测试用户",
			createTime: "2024-06-04",
			content: "",
			replyCount: 2,
			hasLiked: false,
			likeCount: 5,
			replyToComment: {
			  userName: "用户A",
			  content: "这是回复的内容。"
			}
		},
      // 当前tab索引
      curTabIndex: 0,
notificationTypeDesc: '', 
      fromNotification: false, 
      // 发评论
      parentComment: null,
		commentTitle:"最新评论",
      // moment数据
      momentId: 0,
      moment: {
        userName: '云淡风轻',
        userAvatarUrl: '/static/logo.png',
        imgUrl: '',
        content: '',
        createTime: '2024/05/14'
      },
      // comment数据
      commentInfo: {
        commentList: [
          {
            id: 1,
            userAvatarUrl: "/static/image/robot.png",
            userName: "用户1",
            createTime: "2023-01-01",
            content: "好吃",
            replyCount: 2,
            hasLiked: false,
            likeCount: 5,
            replyToComment: {
              userName: "用户A",
              content: "这是回复的内容。"
            }
          },
          {
            id: 2,
            userAvatarUrl: "/static/image/robot.png",
            userName: "用户2",
            createTime: "2023-01-02",
            content: "还不错。",
            replyCount: 0,
            hasLiked: true,
            likeCount: 3
          },
          {
            id: 3,
            userAvatarUrl: "/static/image/robot.png",
            userName: "用户3",
            createTime: "2023-01-03",
            content: "下次不来了",
            replyCount: 1,
            hasLiked: false,
            likeCount: 1
          }
        ],
        pageNo: 1,
        pageSize: 20
      },
      // repost数据
      repostInfo: {
        repostUserList: [],
        pageNo: 1,
        pageSize: 20
      },
      // like数据
      likeInfo: {
        likeUserList: [],
        pageNo: 1,
        pageSize: 20
      }
    };
  },

  onLoad(options) {
    if (options.detail) {
      const detail = JSON.parse(decodeURIComponent(options.detail));
      this.moment.imgUrl = detail.cover;
      this.moment.content = detail.content;
    }
  },

  methods: {
	  async publishComment() {
	  		if (!this.newComment.content) {
	  			uni.showToast({
	  				title: "评论不能为空",
	  				duration: 2000
	  			});
	  			return;
	  		}
	  
	  
	  		uni.showLoading({
	  			title: '发布中...'
	  		});
	  	
	  uni.getSystemInfo({
	        success: (res) => {
				const date = new Date(res.currentDate)
	           this.newComment.createTime= this.formatDate(date)
			 
	        }
	      })
	  		
	  this.commentInfo.commentList.push(this.newComment); 
	  		uni.showToast({
	  			title: "发布成功",
	  			duration: 2000
	  		});
	  	},
	
    // 界面相关函数
    // 点击tab导航
    clikeTabNav(e) {
      let newIndex = e.currentTarget.dataset.index;
      this.updateCurTabIndex(newIndex);
    },

    // 左右滑屏
    swiperChange(e) {
      console.log('swiperChange：' + e.target.source);
      if (e.target.source !== 'touch') {
        return;
      }

      let newIndex = e.target.current;
      newIndex = newIndex > this.curTabIndex ? this.curTabIndex + 1 : this.curTabIndex - 1;
      this.updateCurTabIndex(newIndex);
    },

    updateCurTabIndex(newIndex) {
      newIndex !== this.curTabIndex && (this.curTabIndex = newIndex);
    },

    // 数据相关函数
    // 加载评论数据
    async loadCommentInfo() {
      // let [commentListData] = await httpUtils.postJson('/comment/queryMomentComment', {
      // 	momentId: this.momentId,
      // 	parentId: 0,
      // 	pageNo: this.commentInfo.pageNo,
      // 	pageSize: this.commentInfo.pageSize
      // });
      // this.commentInfo.commentList = this.commentInfo.commentList.concat(commentListData.body);
      this.commentInfo.pageNo++;
    },
    // 加载转发数据
    async loadRepostInfo() {
      let [repostUserData] = await httpUtils.postJson('/repost/queryRepostUser', {
        repostMomentId: this.momentId,
        pageNo: this.repostInfo.pageNo,
        pageSize: this.repostInfo.pageSize
      });
      this.repostInfo.repostUserList = this.repostInfo.repostUserList.concat(repostUserData.body);
      this.repostInfo.pageNo++;
    },
    // 加载点赞数据
    async loadLikeInfo() {
      let [likeUserData] = await httpUtils.postJson('/like/queryLikeUser', {
        targetType: 'MOMENT',
        targetId: this.momentId,
        pageNo: this.likeInfo.pageNo,
        pageSize: this.likeInfo.pageSize
      });
      this.likeInfo.likeUserList = this.likeInfo.likeUserList.concat(likeUserData.body);
      this.likeInfo.pageNo++;
    }
  }
};
</script>

<style scoped>
.line {
  height: 2rpx;
  background-color: #f3f3f3;
  margin-top: -4rpx;
}

/* tab导航 */
.tab-nav-main-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: center;
  border-bottom: 2rpx solid #f3f3f3;
  margin-top: 32rpx;
}

.tab-nav-main {
  display: inline-flex;
  flex-direction: row;
  justify-content: center;
  position: relative;
}

.tab-nav-list {
  display: flex;
  flex-direction: row;
  justify-content: center;
  font-size: 0;
}

.tab-nav-item {
  box-sizing: border-box;
  font-size: 30rpx;
  padding-bottom: 20rpx;
  padding-left: 6rpx;
  padding-right: 6rpx;
  margin: 0 74rpx;
  position: relative;
}
.comment-publish-area {
		background-color: white;
		width: 690rpx;
		padding: 0 30rpx;
		position: fixed;
		bottom: 0;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 90rpx;
		border-top: 1px solid #f3f3f3;
		font-size: 28rpx;
	}

	.comment-publish-input-wrapper {
		background-color: white;
		height: 90rpx;
		max-height: 180rpx;
		width: 500rpx;
		overflow-y: auto;
	}

	.comment-publish-input {
		background-color: white;
		height: 90rpx;
		width: 500rpx;
		color: #333;
		font-size: 28rpx;
		padding-top: 26rpx;
	}

	.comment-publish-button-gray {
		color: #A1A1A1;
	}
.tab-nav-item-num {
  font-size: 20rpx;
  color: #9a9a9a;
  position: absolute;
  right: -20rpx;
  top: 7rpx;
}

.tab-nav-item-active {
  font-weight: bold;
  color: #f6423d;
  border-bottom: 4rpx solid #f6423d;
}

.tab-nav-underline {
  position: absolute;
  bottom: -2rpx;
  left: 8rpx;
  height: 4rpx;
  border-radius: 2rpx;
  background-color: #f6423d;
}

.tab-nav-underline-animation {
  transition-duration: 200ms;
  transition-property: left;
}

/* tab内容 */
.tab-content {
  height: 100%;
}

.tab-content-item {
  overflow-y: auto;
}

/* moment样式 */
.moment-container {
  display: flex;
  align-items: center;
}

.moment-avatar {
  width: 4em;
  height: 4em;
  border-radius: 50%;
  margin-right: 1em;
}

.moment-user-time-wrapper {
  display: flex;
  flex-direction: column;
}

.moment-user-name-wrapper {
  display: flex;
  align-items: center;
}

.moment-user-name,
.moment-notification-type {
  font-size: 2em;
}

.moment-create-time {
  font-size: 1.5em;
  color: #888;
}

.moment-create-time-right {
  text-align: right;
}

.moment-text-content {
  margin-top: 1em;
  font-size: 1.5em;
  line-height: 1.6;
  color: #333;
}

.moment-img {
  margin-top: 1em;
  width: 100%;
  border-radius: 8px;
}

/* 评论样式 */
.comment-list {
  margin-top: 20rpx;
}

.comment-title {
  font-size: 30rpx;
  margin-bottom: 20rpx;
}

.comment-root {
  margin-bottom: 20rpx;
}

.comment-wrapper {
  display: flex;
  flex-direction: row;
}

.comment-avatar {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  margin-right: 20rpx;
}

.comment-info {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.comment-user-name {
  font-size: 30rpx;
}

.comment-create-time {
  font-size: 20rpx;
  color: #888;
}

.comment-content {
  font-size: 28rpx;
  margin-top: 10rpx;
}

.comment-reply-count {
  font-size: 20rpx;
  color: #888;
  margin-top: 10rpx;
}

.comment-reply-to {
  font-size: 20rpx;
  color: #888;
  margin-top: 10rpx;
}

.comment-reply-to-user-name {
  color: #333;
}

.comment-like-wrapper {
  display: flex;
  align-items: center;
  margin-top: 10rpx;
}

.comment-like-text-active {
  color: #f6423d;
}

.comment-like-image {
  width: 24rpx;
  height: 24rpx;
  margin-left: 10rpx;
}

.comment-occupy-position-wrapper {
  margin-top: 20rpx;
}

.comment-occupy-position {
  height: 2rpx;
  background-color: #f3f3f3;
}
</style>
