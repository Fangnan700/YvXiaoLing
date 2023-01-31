<template>
	<view>
		<!-- 顶部滚动栏 -->
		<uni-notice-bar show-icon scrollable text="欢迎使用喻小灵~本工具仅用于学习交流,请勿用作非法用途" />
		</uni-section>

		<!-- 用户输入 -->
		<view v-show="!parse_status">
			<uni-section title="抖音视频直链解析" subTitle="使用抖音的分享链接,解析高清无水印视频提供下载" type="line" padding>
				<uni-easyinput type="textarea" autoHeight v-model="share_url_text" placeholder="请输入抖音分享链接">
				</uni-easyinput>
			</uni-section>
			<button class="button" type="primary" @click="parse">开始解析</button>
			<button class="button" type="default" @click="init">重置</button>
		</view>

		<!-- 视频预览区 -->

		<view class="player_frame" v-show="parse_status">
			<view>
				视频名称：{{ video_name }}
			</view>
			<view>
				视频作者：{{ video_author }}
			</view>
			<video class="player" id="player" :src="video_url" :poster="video_poster"></video>
			<button class="btn" type="primary" @click="download">下载视频</button>
			<button class="btn" type="default" @click="init">换一个</button>
			<view class="progress-box">
				<progress :percent="download_prosess" show-info stroke-width="3" />
			</view>
		</view>

		<!-- 使用说明区 -->
		<view v-show="!parse_status">
			<uni-section title="使用说明" type="line">
				<uni-card :is-shadow="false">
					<text class="uni-body">这是一个抖音无水印视频解析下载工具，将抖音的分享链接粘贴在上方（无需去除文字，会自动去除），点击“开始解析”即可。</text>
				</uni-card>
			</uni-section>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				share_url_text: "",
				share_url: "",
				video_url: "",
				video_poster: "",
				video_name: "",
				video_author: "",
				save_path: "",
				download_prosess: 0,
				parse_status: false
			}
		},
		methods: {
			init() {
				this.parse_status = false;
				this.share_url_text = "";
				this.video_context = uni.createVideoContext("player");
				this.video_context.pause();
			},
			parse() {
				if(this.share_url_text === "") {
					uni.showToast({
						title: '输入不能为空哦~',
						icon: 'none',
						duration: 2000,
						mask: true
					})
				} else {
					this.share_url = this.share_url_text.match(/(https?:\/\/[^\s]+)/g)[0];
					if(this.share_url.substring(0, 20) !== "https://v.douyin.com") {
						uni.showToast({
							title: '这好像不是抖音的分享链接耶~',
							icon: 'none',
							duration: 2000,
							mask: true
						})
					} else {
						// 这里使用第三方接口(作者：Hmily， 微信：tangmin903)
						uni.showLoading({
							title: "正在解析"
						});
						uni.request({
							method: 'POST',
							url: "https://www.hmily.vip/api/dy/?url=" + this.share_url,
							header: {
								"Access-Control-Allow-Origin": "*",
								"user-agent": "user-agent=Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36"
							},
							success: (res) => {
								this.video_url = res.data["video_url"];
								this.video_poster = res.data["video_cover"];
								this.video_name = res.data["desc"];
								this.video_author = res.data["nickname"];
								this.parse_status = true;
								uni.hideLoading();
							}
						});
					}
				}
			},
			download() {
				console.log("下载:" + this.video_url);

				const downloadTask = uni.downloadFile({
					url: this.video_url,
					success: (res) => {
						if (res.statusCode === 200) {
							uni.saveVideoToPhotosAlbum({
								filePath: res.tempFilePath,
								success: function() {
									uni.showToast({
										title: '保存成功',
										icon: 'none',
										duration: 2000,
										mask: true
									});
								},
								fail: function() {
									this.loadProgress = 0;
									uni.showToast({
										title: '保存失败',
										icon: 'none'
									});
								}

							})
						}
					}
				});

				downloadTask.onProgressUpdate((res) => {
					this.download_prosess = res.progress;
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

	.player_frame {
		margin: 10px;

		.player {
			width: 100%;
		}
	}

	.btn {
		margin-top: 10px;
	}
</style>
