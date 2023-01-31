<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<!-- 用户输入 -->
		<view>
			<uni-section title="有道翻译" subTitle="中英文互译工具" type="line" padding>
				<uni-easyinput type="textarea" autoHeight v-model="input_text" placeholder="请输入内容">
				</uni-easyinput>
			</uni-section>
			<button class="button" type="primary" @click="parse">翻译</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>

		<!-- 翻译信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="翻译结果" type="line">
				<uni-card :is-shadow="false">
					<span @longpress="copyText">{{ output_text }}</span>
				</uni-card>
			</uni-section>
		</view>

		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个有道翻译提供的中英文互译接口，输入要翻译的内容即可。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				input_text: "",
				output_text: "",
				lookups_status: false
			}
		},
		methods: {
			init() {
				this.input_text = "";
				this.output_text = "";
				this.lookups_status = false;
			},
			copyText() {
				uni.setClipboardData({
					data: this.output_text,
					success() {
						uni.showToast({
							title: '已复制到剪贴板',
							icon: 'none',
							position: 'top'
						})
					}
				})
			},
			parse() {
				// 使用接口格式（https://v.api.aa1.cn/api/api-fanyi-yd/index.php?msg=我爱你&type=3）
				if (this.input_text !== "") {
					uni.showLoading({
						title: "正在翻译"
					});
					uni.request({
						method: "GET",
						url: "https://v.api.aa1.cn/api/api-fanyi-yd/index.php?msg=" + this.input_text + "&type=3",
						success: (res) => {
							this.output_text = res.data["text"];
							this.lookups_status = true;
							uni.hideLoading();
							uni.showToast({
								title: '翻译成功',
								icon: 'none',
								duration: 2000,
								mask: true
							});
						},
						fail: () => {
							uni.hideLoading();
							uni.showToast({
								title: '翻译失败，请稍后重试',
								icon: 'none',
								duration: 2000,
								mask: true
							});
							this.lookups_status = false;
						}
					})
				} else {
					uni.showToast({
						title: "输入不能为空"
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	.button {
		margin-left: 10px;
		margin-right: 10px;
		margin-bottom: 10px;
	}
</style>
