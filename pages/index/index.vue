<template>
	<view class="content">
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<uni-card title="简介">
			<text class="uni-body">
				{{ introduce_text }}
			</text>
		</uni-card>

		<uni-card title="v1.0.0功能概览">
			<text class="uni-body" v-for="(item, index) in function_list" v-if="item.version === 'v1.0.0'">
				<p>{{ item.id }} 、 {{ item.function }}</p>
			</text>
		</uni-card>

	</view>
</template>

<script>
	export default {
		onLoad:function(){
			uni.showModal({
				title: "提示",
				content: "此App仍处于测试阶段，若出现功能无法使用，请与喻灵联系更新",
				confirmText: "我已明白"
			})
		},
		data() {
			return {
				introduce_text: "【喻小灵】是一款强大的小工具类App，它集成了各种常用的工具功能，能够满足你日常生活和工作中的需求。主要功能包括视频直链解析、IP查询、文案生成等，使用简单、快捷，能帮助你快速解决问题。",
				function_list: [
					{
						"version": "v1.0.0",
						"id": "0",
						"function": "摸鱼专用短视频"
					},
					{
						"version": "v1.0.0",
						"id": "1",
						"function": "抖音无水印视频直链解析下载"
					},
					{
						"version": "v1.0.0",
						"id": "2",
						"function": "快手无水印视频直链解析下载"
					},
					{
						"version": "v1.0.0",
						"id": "3",
						"function": "今日新闻"
					},
					{
						"version": "v1.0.0",
						"id": "4",
						"function": "快递/物流信息查询"
					},
					{
						"version": "v1.0.0",
						"id": "5",
						"function": "有道翻译"
					},
					{
						"version": "v1.0.0",
						"id": "6",
						"function": "IP信息查询"
					},
					{
						"version": "v1.0.0",
						"id": "7",
						"function": "本机IP查询"
					},
					{
						"version": "v1.0.0",
						"id": "8",
						"function": "网站测速工具"
					},
					{
						"version": "v1.0.0",
						"id": "9",
						"function": "爱情文案生成器"
					},
					{
						"version": "v1.0.0",
						"id": "10",
						"function": "添狗日记"
					},
					{
						"version": "v1.0.0",
						"id": "11",
						"function": "骚扰电话查询"
					}
					
				]
			}
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		height: 100%;
	}
</style>
