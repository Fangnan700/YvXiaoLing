<template>
	<view class="content">

		<uni-card :title="name" :sub-title="maxim" thumbnail="/static/me.jpg">
			<text class="uni-body">
				<h3>简介</h3>
				<p>{{ self_introduce }}</p>
				<br>
				<h3>个人博客</h3>
				<p><uni-link :href="self_blog">{{ self_blog }}</uni-link></p>
				<br>
				<h3>GitHub</h3>
				<p><uni-link :href="self_github">{{ self_github }}</uni-link></p>
				<br>
				<h3>CSDN</h3>
				<p><uni-link :href="self_csdn">{{ self_csdn }}</uni-link></p>
				<br>
				<h3>友情链接</h3>
				<p>App所采用的大部分api均来自<uni-link :href="xiarou_link">{{ xiarou }}</uni-link>，感谢夏柔大佬为开源作出的贡献！</p>
				<br>
				<h3>联系喻灵</h3>
				<p>QQ:{{' '}}<span @click="copy_qq">{{ self_qq }}(点击复制)</span></p>
				<p>微信:{{' '}}<span @click="copy_wechat">{{ self_wechat }}(点击复制)</span></p>
				<br>
				
			</text>
		</uni-card>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				name: "喻灵",
				maxim: "一个佛系的程序员。",
				self_introduce: "喻灵是一名计算机科学与技术的大二学生（2023），开发此App的初衷是为了从中学习到开发、运维的相关知识。希望“喻小灵”能为你的工作生活带来便利。",
				self_blog: "https://www.yvling.top",
				self_github: "https://github.com/Fangnan700",
				self_csdn: "https://blog.csdn.net/YV_LING",
				self_qq: "2621737589",
				self_wechat: "f2621737589",
				xiarou_link: "https://api.aa1.cn/",
				xiarou: "“夏柔公益”"
			}
		},
		methods: {
			copy_qq() {
				uni.setClipboardData({
					data: this.self_qq,
					success() {
						uni.showToast({
							title: "已复制QQ号"
						})
					}
				})
			},
			copy_wechat() {
				uni.setClipboardData({
					data: this.self_wechat,
					success() {
						uni.showToast({
							title: "已复制微信号"
						})
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	.content {
		width: 100%;
		height: 100%;
	}
</style>
