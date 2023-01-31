<template>
	<view class="content">
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<uni-search-bar @confirm="search" @clear="clear" v-model="tools_key" cancelButton="none" placeholder="想怎么摸鱼呢？搜一搜看看">
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
				tools_list: [
					{
						"id": "1",
						"name": "随机小姐姐短视频",
						"icon": "static/girl.png",
						"describe": "随机播放从抖音爬取的各种小姐姐视频",
						"page_link": "/pages/girl/girl"
					},
					{
						"id": "2",
						"name": "随机小哥哥短视频",
						"icon": "static/boy.png",
						"describe": "随机播放从抖音爬取的各种小哥哥视频",
						"page_link": "/pages/boy/boy"
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
