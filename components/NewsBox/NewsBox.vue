<template>
	<view class="news-box">
		<view class="pic">
			<image mode="aspectFill" :src="item.picurl"></image>
		</view>
		<view class="text">
			<view class="title">{{item.title}}</view>
			<view class="info" v-if="!item.looktime">
				<text>{{item.author}}</text>
				<text>{{item.hits}}浏览</text>
			</view>
			<view class="info" v-else>
				<text>浏览时间：{{item.looktime}}</text>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"NewsBox",
		props: {
			item: {
				type: Object,
				default() {
					return {
						title: "组件默认的标题",
						author: "张山",
						hits: 668,
						picurl: "/static/image/nopic.jpg",
						looktime: "2023-10-19 11:44:00"
					}
				}
			}
		},
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss">
.news-box {
	display: flex;
	.pic {
		width: 230rpx;
		height: 160rpx;
		image {
			width: 100%;
			height: 100%;
		}
	}
	.text {
		flex: 1;
		padding-left: 20rpx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		.title {
			font-size: 30rpx;
			color: #333;
			// 内容超过两行显示省略号
			overflow: hidden; // 溢出内容隐藏
			text-overflow: ellipsis; // 文本溢出部分用省略号表示
			display: -webkit-box; // 特别显示模式
			-webkit-line-clamp: 2; // 行数
			-webkit-box-orient: vertical; // 盒子中内容垂直排列
			
		}
		.info {
			font-size: 26rpx;
			color: #999;
			text {
				padding-right: 30rpx;
			}
		}
	}
}
</style>