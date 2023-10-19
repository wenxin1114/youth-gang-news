<template>
	<view class="home">
		<view class="scrollNav">
			<scroll-view scroll-x class="navscroll" >
				<view class="item" :class="navIndex === index ? 'active' : ''" v-for="(item,index) in navTitleList" @click="clickNav(index, item.id)" :key="item.id">{{item.classname}}</view>
			</scroll-view>
		</view>
		<view class="content">
			<div class=row v-for="item in newsList" :key="item.id">
				<NewsBox :item="item" @click.native="goDetail(item.id)"></NewsBox>
			</div>
		</view>
		
		<view class="nodata" v-if="!newsList.length">
			<image src="../../static/image/nodata.png" mode=""></image>
		</view>
		<view class="loading" v-else>
			<view v-if="loading === 1">数据加载中...</view>
			<view v-else-if="loading === 2">没有更多数据了</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navTitleList: [],
				newsList: [],
				cid: 50,
				currentPage: 1,
				loading: 0  // 0 默认 1加载中 2没有更多了
			}
		},
		methods: {
			// 获取新闻栏目
			getNavTitleList() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: (res) => {
						this.navTitleList = res.data;
					}
				})
			},
			// 获取新闻列表
			getNewsList() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: this.cid,
						page: this.currentPage,
					},
					success: (res) => {
						if (res.data.length === 0) {
							this.loading = 2;
						} else {
							this.newsList = [...this.newsList, ...res.data];
						}
						
					}
				})
			},
			// 点击新闻标题栏
			clickNav(index, cid) {
				this.navIndex = index;
				this.currentPage = 1;
				this.newsList = [];
				this.loading = 0;
				this.cid = cid;
				this.getNewsList();
			},
			// 跳转详情页
			goDetail(id) {
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${this.cid}&id=${id}`,
				})
			}
		},
		onLoad() {
			this.getNavTitleList();
			this.getNewsList()
		},
		onReachBottom() {
			// 如果 loading 为 2，说明没有数据了，就不发送请求
			if (this.loading === 2) {
				return;
			}
			// 触底后页面增加 1
			this.currentPage++;
			this.loading = 1;
			this.getNewsList()
		}
	}
</script>

<style lang="scss" scoped>
.navscroll {
	height: 100rpx;
	background: #F7F8FA;
	white-space: nowrap;
	position: fixed;
	top: var(--window-top);
	left: 0;
	z-index: 10;
	// 移除H5端滚动条
	/deep/::-webkit-scrollbar {
		width: 4px !important;
		height: 1px !important;
		overflow: auto !important;
		background: transparent !important;
		-webkit-appearance: auto !important;
		display: block;
	}
	.item {
		font-size: 40rpx;
		display: inline-block;
		line-height: 100rpx;
		padding: 0 30rpx;
		color: #333;
		&.active {
			color: #31C27C;
		}
	}
}
.content {
	padding: 30rpx;
	padding-top: 130rpx;
	.row {
		border-bottom: 1px dotted #efefef;
		padding: 15rpx 0;
	}
}
.nodata {
	display: flex;
	justify-content: center;
	image {
		width: 360rpx;
	}
}
.loading {
	text-align: center;
	font-size: 26rpx;
	color: #888;
	line-height: 2em;
}
</style>
