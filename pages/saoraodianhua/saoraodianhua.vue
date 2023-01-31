<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<!-- 用户输入 -->
		<view>
			<uni-section title="骚扰电话查询" subTitle="输入手机号码,查看是不是骚扰电话" type="line" padding>
				<uni-easyinput type="textarea" autoHeight v-model="phone_text" placeholder="请输入手机号码">
				</uni-easyinput>
			</uni-section>
			<button class="button" type="primary" @click="parse">开始查询</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>

		<!-- 查询信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="查询结果" type="line">
				<uni-card :is-shadow="false">
					<view>提示信息：{{ msg }}</view>
					<view>手机号码：{{ tel }}</view>
					<view>所属省份：{{ province }}</view>
					<view>所属城市：{{ city }}</view>
					<view>运营商：{{ operator }}</view>
					<view>
						安全信息：
						<view v-for="(item, index) in infos">
							<strong>来源：</strong>{{ item.name }}<br><strong>状态：</strong>{{  item.msg }}
						</view>
					</view>
				</uni-card>
			</uni-section>
		</view>

		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个骚扰电话查询工具，将手机号码粘贴在上方，点击“开始查询”即可查询该号码的归属地、是否被判定为骚扰电话等信息。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				phone_text: "",
				msg: "",
				tel: "",
				province: "",
				city: "",
				operator: "",
				infos: "",
				lookups_status: false
			}
		},
		methods: {
			init() {
				this.phone_text = "";
				this.lookups_status = false;
			},
			parse() {
				// 使用接口格式（https://zj.v.api.aa1.cn/api/saorao/?phone=13132131321）
				uni.showLoading({
					title: "正在查询"
				});
				uni.request({
					method: "GET",
					url: "https://zj.v.api.aa1.cn/api/saorao/?phone=" + this.phone_text,
					success: (res) => {
						if(res.data.success) {
							this.msg = "查询成功";
							this.tel = res.data.tel;
							this.province = res.data.info["province"];
							this.city = res.data.info["city"];
							this.operator = res.data.info["operator"];
							this.infos = res.data.data;
							this.lookups_status = true;
							uni.hideLoading();
						} else {
							this.msg = res.data.message;
							this.lookups_status = true;
							uni.hideLoading();
						}
					},
					fail: () => {
						this.msg = "查询失败"
						uni.hideLoading();
						uni.showToast({
							title: '查询失败，请稍后重试',
							icon: 'none',
							duration: 2000,
							mask: true
						});
						this.lookups_status = false;
					}
				})
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
