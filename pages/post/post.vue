

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
			<image class="moment-img"  :src="moment.imgUrl" mode="aspectFill"></image>
		</view>
		<view class="tab-nav-main-wrapper">
			<view class="tab-nav-main">
				<view class="tab-nav-list" ref="tabNavList">
					<view class="tab-nav-item" :class="curTabIndex == 0 ? 'tab-nav-item-active' : ''" :data-index="0" @click="clikeTabNav">
						评论
						<text class="tab-nav-item-num">{{ moment.commentCount }}</text>
					</view>
					<view class="tab-nav-item" :class="curTabIndex == 1 ? 'tab-nav-item-active' : ''" :data-index="1" @click="clikeTabNav">
						转发
						<text class="tab-nav-item-num">{{ moment.repostCount }}</text>
					</view>
					<view class="tab-nav-item" :class="curTabIndex == 2 ? 'tab-nav-item-active' : ''" :data-index="2" @click="clikeTabNav">
						赞
						<text class="tab-nav-item-num">{{ moment.likeCount }}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="line"></view>
		<swiper class="tab-content" :duration="300" :current="curTabIndex" @change="swiperChange" acceleration="false">
			<swiper-item class="tab-content-item">
				<commentList :commentList="commentInfo.commentList" :parentId="parseInt('0')" title="最新评论"></commentList>
			</swiper-item>
			<swiper-item class="tab-content-item">
				<momentDetailRepost v-for="(repostUser, index) in repostInfo.repostUserList" :repostUser="repostUser" :index="index"></momentDetailRepost>
			</swiper-item>
			<swiper-item class="tab-content-item">
				<momentDetailLike v-for="(likeUser, index) in likeInfo.likeUserList" :likeUser="likeUser" :index="index"></momentDetailLike>
			</swiper-item>
		</swiper>

		<publishComment :parentComment="parentComment" :momentId="moment.id"></publishComment>
	</view>
</template>

<script>
import httpUtils from '../../common/util/httpUtils.js';
import moment from '../../components/moment/moment.vue';
import commentList from '../../components/commentList/commentList.vue';
import publishComment from '../../components/publishComment/publishComment.vue';
import momentDetailRepost from '../../components/momentDetailRepost/momentDetailRepost.vue';
import momentDetailLike from '../../components/momentDetailLike/momentDetailLike.vue';

export default {
	components: {
		moment,
		commentList,
		publishComment,
		momentDetailRepost,
		momentDetailLike
	},

	data() {
		return {
			// 当前tab索引
			curTabIndex: 0,

			// 发评论
			parentComment: null,

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
				commentList: [],
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
		console.log(1);
		console.log(options);
	  if (options.detail) {
	   const detail = JSON.parse(decodeURIComponent(options.detail));
	    console.log(detail.content);
		this.moment.imgUrl = detail.cover;
		 this.moment.content =detail.content;
	  }
	},

	methods: {
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
			let [commentListData] = await httpUtils.postJson('/comment/queryMomentComment', {
				momentId: this.momentId,
				parentId: 0,
				pageNo: this.commentInfo.pageNo,
				pageSize: this.commentInfo.pageSize
			});
			this.commentInfo.commentList = this.commentInfo.commentList.concat(commentListData.body);
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

<style>
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
@import url('moment.css');
@import url('parse.css');
.moment-container {
	display: flex;
	align-items: center;
}

.moment-avatar {
	width: 4em; /* 假设字体大小为2em，这里设置为2倍 */
	height: 4em;
	border-radius: 50%; /* 如果需要圆形头像 */
	margin-right: 1em; /* 头像和文本之间的间距 */
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
	font-size: 2em; /* 设置字体大小 */
}

.moment-create-time {
	font-size: 1.5em; /* 设置时间的字体大小 */
	color: #888; /* 可以根据需求设置颜色 */
}

.moment-create-time-right {
	text-align: right;
}
.moment-container {
	display: flex;
	align-items: center;
}

.moment-avatar {
	width: 4em; /* 假设字体大小为2em，这里设置为2倍 */
	height: 4em;
	border-radius: 50%; /* 如果需要圆形头像 */
	margin-right: 1em; /* 头像和文本之间的间距 */
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
	font-size: 2em; /* 设置字体大小 */
}

.moment-create-time {
	font-size: 1.5em; /* 设置时间的字体大小 */
	color: #888; /* 可以根据需求设置颜色 */
}

.moment-create-time-right {
	text-align: right;
}

.moment-text-content {
	margin-top: 1em; /* 上方的间距 */
	font-size: 1.5em; /* 设置动态内容的字体大小 */
	line-height: 1.6; /* 行高 */
	color: #333; /* 文本颜色 */
}

.moment-img {
	margin-top: 1em; /* 上方的间距 */
	width: 100%; /* 宽度为容器宽度 */
	border-radius: 8px; /* 可选：图片圆角 */
}
</style>
