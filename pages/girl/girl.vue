<template>
	<view class="player_frame">
		<!-- <video class="player" src="//ml.v.api.aa1.cn/girl-11-02//video/艾特你身边脑子里全是女人的兄弟-面纱变装--了不起的中国成分2---抖音.mp4"></video> -->

		<video class="player" id="_player" :src="current_video" autoplay="true" loop="loop" @error="handleError"></video>
		<button class="btn-default" @click="change">换一个</button>

	</view>
</template>

<script>
	export default {
		onLoad: function() {
		    this._player = uni.createVideoContext('_player');
			uni.request({
				url: "https://v.api.aa1.cn/api/api-girl-11-02/index.php?type=json",
				success:function(res){
					let _tmp = res.data["mp4"];
					this.current_video = _tmp.replace(/免费api:api.aa1.cn/g, "");
				}
			})
		},
		onHide: function() {
			this._player.pause();
		},
		data() {
			return {
				_player: null,
				current_video: ""
			}
		},
		methods: {
			change() {
				uni.request({
					// 暂时只使用夏柔的一个接口，后续可以增加
					url: "https://v.api.aa1.cn/api/api-girl-11-02/index.php?type=json",
					success: (res) => {
						let _tmp = res.data["mp4"];
						this.current_video = _tmp.replace(/免费api:api.aa1.cn/g, "");
					}
				})
			},
			handleError() {
				this.change();
			},
		}
	}
</script>

<style lang="scss">
	.player_frame {
		width: 100%;
		height: 100%;
		overflow: hidden;

		.player {
			width: 100%;
			height: 92%;
		}


	}
</style>
