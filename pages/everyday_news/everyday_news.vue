<template>
	<view class="content">
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<!-- 查询信息展示 -->
		<uni-section title="新闻概览" subTitle="点击图片查看大图" type="line" padding v-show="lookups_status"></uni-section>
		<view class="show" v-show="lookups_status">
			<image :src="news_img" mode="aspectFit" @click="previewImg"></image>
		</view>

		<!-- 用户输入 -->
		<view>
			<uni-section title="今日新闻" subTitle="红色背景的正式新闻模板,打印当日新闻" type="line" padding></uni-section>
			<button class="button" type="primary" @click="parse">生成</button>
		</view>

		<!-- 使用说明区 -->
		<view>
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个红色背景的今日新闻打印生成工具，点击“生成”即可获取排好版的当日新闻。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	import {
		pathToBase64,
		base64ToPath
	} from 'image-tools';
	const Buffer = require('buffer').Buffer;
	export default {
		data() {
			return {
				news_img: "",
				lookups_status: false,
			}
		},
		methods: {
			previewImg() {
				let that = this;
				uni.showLoading({
					title: "加载中..."
				})
				base64ToPath(this.news_img).then(path => {
					let imgs = [];
					uni.hideLoading()
					imgs[0] = path;
					uni.previewImage({
						current: 0,
						urls: imgs
					});
				})
				.catch(error => {
					// 图片加载失败
					uni.hideLoading()
				})
			},
			parse() {
				uni.showLoading({
					title: "正在加载"
				});
				// 这里使用夏柔公益的api接口，接口格式（https://zj.v.api.aa1.cn/api/60s-v2/?cc=喻灵）
				uni.request({
					url: "https://zj.v.api.aa1.cn/api/60s-v2/?cc=喻灵",
					responseType: "arraybuffer",
					success: (res) => {
						let arrayBuffer = res.data;
						//将ArrayBuffer转换为base64
						let base64 = this.arrayBufferToBase64(arrayBuffer);
						//使用base64处理图片流
						this.news_img = "data:image/png;base64," + base64;
						this.lookups_status = true;
						uni.hideLoading();
					}
				});
			},
			arrayBufferToBase64(buffer) {
				let binary = '';
				let bytes = new Uint8Array(buffer);
				let len = bytes.byteLength;
				for (let i = 0; i < len; i++) {
					binary += String.fromCharCode(bytes[i]);
				}
				return Buffer.from(binary, 'binary').toString('base64');
			}

		}
	}
</script>

<style lang="scss">
	.content {
		.show {
			width: 100%;
			text-align: center;
		}

		.button {
			margin-left: 10px;
			margin-right: 10px;
			margin-bottom: 10px;
		}
	}
</style>
