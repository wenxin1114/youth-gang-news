<template>
	<view class="detail">
		<view class="title">
			{{detail.title}}
		</view>
		<view class="info">
			<view class="author">
				编辑：{{detail.author}}
			</view>
			<view class="time">
				发布日期：{{detail.posttime}}
			</view>
		</view>
		<view class="content">
			<!-- 富文本 -->
			<rich-text :nodes="detail.content"></rich-text>
		</view>
		<view class="description">
			声明:本站的内容均采集与腾讯新闻，如果侵权请联系管理(1731551615@qq.com)进行整改除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持
		</view>
	</view>
</template>

<script>
	import {formatDate} from '@/utils/utils.js'
	export default {
		data() {
			return {
				detail: {}
			}
		},
		methods: {
			getDetail(data) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/detail.php",
					data,
					success: (res) => {
						res.data.content = res.data.content.replace(/<img/gi, '<img style="max-width: 100%"');
						res.data.posttime = formatDate(res.data.posttime);
						this.detail = res.data;
						// 保存历史记录
						this.saveHistory()
						uni.setNavigationBarTitle({
							title:this.detail.title
						})
					}
				})
			},
			saveHistory() {
				let historyArr = uni.getStorageSync("historyArr") || []
				let item = {
					id: this.detail.id,
					classid: this.detail.classid,
					picurl: this.detail.picurl,
					title: this.detail.title,
					looktime: formatDate(Date.now().toString())
				}
				let index = historyArr.findIndex(i => {
					return item.id === i.id;
				})

				if (index === -1) {
					if (historyArr.length === 10) {
						historyArr.pop()
					}
					// 不存在，再保存
					historyArr.unshift(item)
					uni.setStorageSync("historyArr", historyArr)
				}
			}
		},
		onLoad(e) {
			this.getDetail(e)
		}
	}
</script>

<style lang="scss">
	.detail {
		padding: 30rpx;

		.title {
			font-size: 40rpx;
			color: #333;
		}

		.info {
			background: #f6f6f6;
			padding: 20rpx 10rpx;
			font-size: 25rpx;
			color: #666;
			display: flex;
			justify-content: space-between;
			margin: 40rpx 0;
		}

		.content {
			padding-bottom: 50rpx;

			/deep/ img {
				max-width: 100%;
			}
		}

		.description {
			background: #FEF0F0;
			font-size: 26rpx;
			padding: 30rpx;
			color: #F89898;
			line-height: 1.8em;
		}
	}
</style>
