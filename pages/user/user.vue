<template>
	<view class="user">
		<view class="top">
			<image src="/static/image/history.png" mode=""></image>
			<view class="text">浏览历史</view>
		</view>
		<view class="content">
			<view class="row" v-for="item in historyArr" :key="item.id">
				<NewsBox :item="item" @click.native="goDetail(item.id)"></NewsBox>
			</view>
		</view>
		<view class="nohis" v-if="!historyArr.length">
			<image src="../../static/image/nohis.png" mode=""></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyArr: []
			}
		},
		methods: {
			// 跳转详情页
			goDetail(id) {
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${this.cid}&id=${id}`,
				})
			},
			getHistoryArr() {
				this.historyArr = uni.getStorageSync("historyArr") || []
			}
		},
		onShow() {
			this.getHistoryArr()
		}
	}
</script>

<style lang="scss">
	.user {
		.top {
			padding: 50rpx 0;
			background: #f8f8f8;
			color: #666;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			image {
				width: 150rpx;
				height: 150rpx;
			}
			.text {
				font-size: 30rpx;
			}
		}
		.content {
			padding: 30rpx;
			.row {
				border-bottom: 1px dotted #efefef;
				padding: 15rpx 0;
			}
		}
		.nohis {
			display: flex;
			justify-content: center;
			image {
				width: 360rpx;
			}
		}
	}
</style>
