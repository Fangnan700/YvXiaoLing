<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>
		
		<!-- 用户输入 -->
		<view>
			<uni-section title="舔狗日记" subTitle="如何舔女神？来看看这里!" type="line" padding></uni-section>
			<button class="button" type="primary" @click="parse">生成</button>
			<button class="button" type="default" @click="init">换一句</button>
		</view>
		
		<!-- 查询信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="舔狗日记" type="line">
				<uni-card :is-shadow="false">
					<text>{{ tiangou_text }}</text>
				</uni-card>
			</uni-section>
		</view>
		
		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个舔狗教学工具，点击“生成”即可输出讨好女神的话语。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tiangou_text: "",
				lookups_status: false,
			}
		},
		methods: {
			init() {
				this.parse();
			},
			parse() {
				uni.showLoading({
					title: "正在加载"
				});
				// 使用接口格式（https://v.api.aa1.cn/api/tiangou/index.php）
				uni.request({
					url: "https://v.api.aa1.cn/api/tiangou/index.php",
					success: (res) => {
						let _tmp = res.data
						this.tiangou_text = _tmp.substring(4, _tmp.length-4);
						this.lookups_status = true;
						uni.hideLoading();
					}
				});
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
