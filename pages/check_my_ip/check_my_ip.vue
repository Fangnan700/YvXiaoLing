<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>
		
		<!-- 用户输入 -->
		<view>
			<uni-section title="本机IP信息查询" subTitle="查询本机IP的国家,地区,城市,运营商等信息" type="line" padding></uni-section>
			<button class="button" type="primary" @click="check">开始查询</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>
		
		<!-- 查询信息展示 -->
		<view v-show="lookups_status">
			<uni-section title="查询结果" type="line">
				<uni-card :is-shadow="false">
					<view>提示信息：{{ msg }}</view>
					<view>本机IP：{{ query }}</view>
					<view>国家：{{ country }}</view>
					<view>国家代码：{{ countryCode }}</view>
					<view>地区：{{ regionName }}</view>
					<view>地区代码：{{ region }}</view>
					<view>城市：{{ city }}</view>
					<view>时区：{{ timezone }}</view>
					<view>运营商：{{ isp }}</view>
					<view>中心经度：{{ lon }}</view>
					<view>中心纬度：{{ lat }}</view>
				</uni-card>
			</uni-section>
		</view>
		
		<!-- 使用说明区 -->
		<view v-show="!lookups_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个本机IP查询工具，点击“开始查询”即可查询本机IP的国家、城市、运营商、区域中心经纬度等信息。</text>
				</uni-card>
			</uni-section>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ip_text: "",
				lookups_status: false,
				country: "",
				countryCode: "",
				region: "",
				regionName: "",
				city: "",
				lat: "",
				lon: "",
				timezone: "",
				isp: "",
				query: "",
				msg: ""
			}
		},
		methods: {
			init() {
				this.ip_text = "";
				this.msg = "";
				this.query = "";
				this.country = "";
				this.countryCode = "";
				this.regionName = "";
				this.region= "";
				this.city = "";
				this.timezone = "";
				this.lon = "";
				this.lat = "";
				this.isp = "";
				this.lookups_status = false;
			},
			check() {
				// 这里使用夏柔公益提供的api接口，用于获取本机ip
				uni.request({
					url: "https://v.api.aa1.cn/api/myip/index.php?aa1=json",
					success:(res) => {
						this.ip_text = res.data["myip"];
						this.parse();
					}
				})
			},
			parse() {
				// 这里使用ip-api.com提供的接口，用于查询ip信息，接口格式（http://ip-api.com/json/103.156.184.84?lang=zh-CN）
				if(this.validateIP(this.ip_text)) {
					uni.showLoading({
						title: "正在查询"
					});
					uni.request({
						method: "GET",
						url: "http://ip-api.com/json/" + this.ip_text + "?lang=zh-CN",
						success:(res) => {
							if(res.data.status === "success") {
								this.msg = "查询成功";
								this.query = res.data["query"];
								this.country = res.data["country"];
								this.countryCode = res.data["countryCode"];
								this.regionName = res.data["regionName"];
								this.region= res.data["region"];
								this.city = res.data["city"];
								this.timezone = res.data["timezone"];
								this.lon = res.data["lon"];
								this.lat = res.data["lat"];
								this.isp = res.data["isp"];
								this.lookups_status = true;
								uni.hideLoading();
							} else {
								this.msg = res.data["message"];
								this.lookups_status = true;
							}
						},
						fail:() => {
							uni.showToast({
								title: '查询失败，请稍后重试',
								icon: 'none',
								duration: 2000,
								mask: true
							});
							this.lookups_status = false;
						}
					})
				} else {
					uni.showToast({
						title: '查询失败，请稍后重试',
						icon: 'none',
						duration: 2000,
						mask: true
					});
					this.lookups_status = false;
				}
			},
			validateIP(ip) {
				// 校验ip是否合法
			    let pattern = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
			    return pattern.test(ip);
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
