<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<!-- 用户输入 -->
		<view>
			<uni-section title="快递/物流信息查询" subTitle="输入快递/物流信息查询,查询物流信息" type="line" padding>
				<uni-easyinput type="textarea" autoHeight v-model="num" placeholder="请输入快递/物流单号,一次只能查询一个">
				</uni-easyinput>
				<uni-data-picker class="selector" placeholder="请选择快递/物流公司" popup-title="请选择快递/物流公司"
					:localdata="companies" v-model="com">
				</uni-data-picker>
			</uni-section>
			<button class="button" type="primary" @click="parse">查询</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>

		<!-- 查询信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="查询结果" type="line">
				<uni-card :is-shadow="false">
					<view>提示信息：{{ msg }}</view>
					<view @longpress="copyText">快递单号：{{ num }}</view>
					<view v-for="(item, index) in companies" v-if="item.value===com">承运公司：{{ item.text }}</view>
					<view class="info" v-for="(item, index) in info_obj">
						<uni-card>
							<span class="item_title">时间：</span>{{ item.time }}<br>
							<span class="item_title">信息：</span>{{ item.context }}<br>
						</uni-card>
					</view>
				</uni-card>
			</uni-section>
		</view>

		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个快递/物流信息查询工具，将快递/物流单号粘贴在上方，点击“开始查询”即可查询物流信息。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	import md5 from 'js-md5';

	export default {
		data() {
			return {
				com: "",
				num: "",
				msg: "",
				lookups_status: false,
				info_obj: "",
				companies: [{
						"text": "顺丰速运",
						"value": "shunfeng"
					},
					{
						"text": "京东物流",
						"value": "jd"
					},
					{
						"text": "圆通速递",
						"value": "yuantong"
					},
					{
						"text": "韵达快递",
						"value": "yunda"
					},
					{
						"text": "中通快递",
						"value": "zhongtong"
					},
					{
						"text": "申通快递",
						"value": "shentong"
					},
					{
						"text": "邮政快递包裹",
						"value": "youzhengguonei"
					},
					{
						"text": "邮政标准快递",
						"value": "youzhengbk"
					},
					{
						"text": "EMS",
						"value": "ems"
					},
					{
						"text": "EMS-国际件",
						"value": "emsguoji"
					},
					{
						"text": "百世快递",
						"value": "huitongkuaidi"
					},
					{
						"text": "极兔速递",
						"value": "jtexpress"
					},
					{
						"text": "丰网速运",
						"value": "fengwang"
					},
					{
						"text": "德邦快递",
						"value": "debangkuaidi"
					},
					{
						"text": "德邦物流",
						"value": "debangwuliu"
					}
				],
				// 接口密钥自行获取
				key_customer: [{
						"key": "CtlkIHWB2380",
						"customer": "75E3FF6A6BB670D65218066CA5FFB616"
					}
				],
				random_index: ""
			}
		},
		methods: {
			init() {
				this.msg = "";
				this.com = "";
				this.num = "";
				this.lookups_status = false;
			},
			copyText() {
				uni.setClipboardData({
					data: this.num,
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

				// 使用快递100的接口（https://poll.kuaidi100.com/poll/query.do）
				uni.showLoading({
					title: "正在查询"
				});

				let base_url = "https://poll.kuaidi100.com/poll/query.do";

				// key和customer只能查询100单，用完即废
				// 这里预置1个账号的密钥对，总共100单的查询量，如需后续使用，自行申请
				this.random_index = Math.floor(Math.random() * this.key_customer.length);
				let key = this.key_customer[this.random_index].key;
				let customer = this.key_customer[this.random_index].customer;

				let param = {
					com: this.com,
					num: this.num,
					phone: "",
					from: "",
					to: "",
					resultv2: ""
				};

				let headers = {
					"Content-Type": "application/x-www-form-urlencoded"
				};

				let postData = {};
				postData.customer = customer;
				postData.param = JSON.stringify(param);
				let sign = md5(postData.param + key + postData.customer);
				postData.sign = sign.toUpperCase();

				let url = 'http://poll.kuaidi100.com/poll/query.do';

				uni.request({
					url: url,
					method: 'POST',
					header: headers,
					data: postData,
					success: (res) => {
						if (res.data.message === "ok") {
							this.msg = "查询成功",
								this.com = res.data.com,
								this.info_obj = res.data.data;
							this.lookups_status = true;
						} else {
							this.msg = res.data.message;
							this.lookups_status = true;
						}

						uni.hideLoading();
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	.item_title {
		font-weight: bold;
	}

	.selector {
		margin-top: 10px;
	}

	.button {
		margin-left: 10px;
		margin-right: 10px;
		margin-bottom: 10px;
	}
</style>
