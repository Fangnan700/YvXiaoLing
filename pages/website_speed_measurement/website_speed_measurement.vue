<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>
		
		<!-- 用户输入 -->
		<view>
			<uni-section title="网站测速工具" subTitle="输入网站地址,测试网站的访问速度" type="line" padding>
				<uni-easyinput type="textarea" autoHeight v-model="url_text" placeholder="请输入网站地址">
				</uni-easyinput>
			</uni-section>
			<button class="button" type="primary" @click="parse">开始测试</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>
		
		<!-- 查询信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="测试结果" type="line">
				<uni-card :is-shadow="false">
					<view>提示信息：{{ msg }}</view>
					<view>网站URL：{{ url_text }}</view>
					<view>网站IP：{{ ip }}</view>
					<view>网站延迟：{{ time }}</view>
					<view>DNS缓存时间：{{ ttl }}</view>
				</uni-card>
			</uni-section>
		</view>
		
		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个网站访问速度测试工具，将网站地址粘贴在上方，点击“开始测试”即可。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url_text: "",
				base_url: "https://ml.v.api.aa1.cn/ping.php?host=",
				lookups_status: false,
				msg: "",
				ip: "",
				time: "",
				ttl: ""
			}
		},
		methods: {
			init() {
				this.url_text = "";
				this.msg = "";
				this.lookups_status = false;
			},
			parse() {
				// 使用接口格式（https://ml.v.api.aa1.cn/ping.php?host=www.yvling.top）
				uni.showLoading({
					title: "正在测试"
				});
				uni.request({
					method: "GET",
					url: this.base_url + this.url_text,
					success:(res) => {
						if(res.data.msg === "ok") {
							this.msg = "测试成功";
							this.ip = res.data["ip"];
							this.time = res.data["time"];
							this.ttl = res.data["ttl"];
							this.lookups_status = true;
							uni.hideLoading();
						} else {
							this.msg = res.data["msg"];
							this.lookups_status = true;
							uni.hideLoading();
						}
					},
					fail:() => {
						uni.showToast({
							title: '测试失败，请稍后重试',
							icon: 'none',
							duration: 2000,
							mask: true
						});
						this.lookups_status = false;
					},
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
