<template>
	<view class="player_frame">

		<video class="player" id="_player" :src="current_video" autoplay="true" loop="loop" @error="handleError"></video>
		<button class="btn-default" @click="change">换一个</button>

	</view>
</template>

<script>
	export default {
		onLoad: function() {
		    this._player = uni.createVideoContext('_player');
			uni.request({
				url: "https://zj.v.api.aa1.cn/api/video_dyv1",
				success:function(res){
					this.current_video = res.data["url"];
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
					// 这里使用来自夏柔公益的api接口
					url: "https://zj.v.api.aa1.cn/api/video_dyv1",
					success: (res) => {
						this.current_video = res.data["url"];
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
