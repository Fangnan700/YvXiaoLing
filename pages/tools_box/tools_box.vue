<template>
	<view class="content">
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<uni-search-bar @confirm="search" @clear="clear" v-model="tools_key" cancelButton="none" placeholder="请输入工具名称/类型">
		</uni-search-bar>

		<!-- 工具列表(非搜索状态) -->
		<view v-show="!search_status">
			<view class="content-nosearch" v-for="(item, index) in tools_list">
				<navigator :url="item.page_link">
					<uni-card class="tools_card" :title="item.name" thumbnail="" extra="" note="Tips">
						<img class="tools_icon" :src="item.icon" alt="">
						<span class="tools_text">{{ item.describe }}</span>
					</uni-card>
				</navigator>
			</view>
		</view>
		
		<!-- 工具列表(搜索状态) -->
		<view v-show="search_status">
			<view class="content-nosearch" v-for="(item, index) in tools_list" v-if="item.name.includes(tools_key.toUpperCase())">
				<navigator :url="item.page_link">
					<uni-card class="tools_card" :title="item.name" thumbnail="" extra="" note="Tips">
						<img class="tools_icon" :src="item.icon" alt="">
						<span class="tools_text">{{ item.describe }}</span>
					</uni-card>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tools_list: [{
						"id": "1",
						"name": "抖音无水印视频直链解析下载",
						"icon": "static/douyin.png",
						"describe": "提取抖音无水印视频直链并提供下载",
						"page_link": "/pages/douyin_straight_chain/douyin_straight_chain"
					},
					{
						"id": "2",
						"name": "快手无水印视频直链解析下载",
						"icon": "static/kuaishou.png",
						"describe": "提取快手无水印视频直链并提供下载",
						"page_link": "/pages/kuaishou_straight_chain/kuaishou_straight_chain"
					},
					{
						"id": "3",
						"name": "今日新闻",
						"icon": "static/news.png",
						"describe": "获取今日新闻",
						"page_link": "/pages/everyday_news/everyday_news"
					},
					{
						"id": "4",
						"name": "快递/物流信息查询",
						"icon": "static/kuaidi.png",
						"describe": "输入快递/物流单号,查询物流信息",
						"page_link": "/pages/kuaidichaxun/kuaidichaxun"
					},
					{
						"id": "5",
						"name": "有道翻译",
						"icon": "static/youdao.png",
						"describe": "中英文互译工具",
						"page_link": "/pages/youdao/youdao"
					},
					{
						"id": "6",
						"name": "IP信息查询",
						"icon": "static/ip.png",
						"describe": "查询IP所属地域等信息,可精确至市级",
						"page_link": "/pages/ip_lookups/ip_lookups"
					},
					{
						"id": "7",
						"name": "本机IP查询",
						"icon": "static/ip.png",
						"describe": "查询本机IP的地区,城市,运营商等信息",
						"page_link": "/pages/check_my_ip/check_my_ip"
					},
					{
						"id": "8",
						"name": "网站测速工具",
						"icon": "static/speed.png",
						"describe": "输入网站地址,测试网站的访问速度",
						"page_link": "/pages/website_speed_measurement/website_speed_measurement"
					},
					{
						"id": "9",
						"name": "爱情文案生成器",
						"icon": "static/love.png",
						"describe": "随机输出一句爱情文案",
						"page_link": "/pages/love_text/love_text"
					},
					{
						"id": "10",
						"name": "舔狗日记",
						"icon": "static/tiangou.png",
						"describe": "如何舔女神？来看看这里!",
						"page_link": "/pages/tiangou/tiangou"
					},
					{
						"id": "11",
						"name": "骚扰电话查询",
						"icon": "static/shouji.png",
						"describe": "输入手机号码,查看是不是骚扰电话",
						"page_link": "/pages/saoraodianhua/saoraodianhua"
					}
				],
				tools_key: "",
				search_status: false
			}
		},
		methods: {
			search() {
				this.search_status = true;
			},
			clear() {
				this.search_status = false;
			}
		},
		computed: {
			
		}
	}
</script>

<style lang="scss">
	.content {
		padding-bottom: 20px;
		.content-nosearch {
			.tools_card {
				.tools_icon {
					width: 30px;
					height: 30px;
				}
		
				.tools_text {
					height: 30px;
					line-height: 30px;
					margin-left: 20px;
				}
			}
		}
	}
</style>
