<template>
	<view>
		<image class="icon-back"  src="../../static/back.png" mode="aspectFit"></image>
		<video id="vedio" src="http://vjs.zencdn.net/v/oceans.mp4" @play="play">
		</video>
		<image class="video-img"  src="../../static/test.png" mode="aspectFit" :class="{'active': isPlay}"></image>
		<image v-if="!isPlay" src="../../static/play.gif" mode="aspectFit" class="play" @click="playFn"></image>
		<image class="to-detial"  src="../../static/xx.png" mode="aspectFit"></image>
		<view class="video-detail">
			<view class="item">品名：烟花</view>
			<view>含量：烟花</view>
			<view>规格：烟花</view>
		</view>
		<view class="video-op">
			<image  src="../../static/202181815576.png" mode="aspectFit" ></image>
			<image  src="../../static/zan.png" mode="aspectFit" ></image>
			<image  src="../../static/zhuanfa.png" mode="aspectFit"></image>
		</view>
	</view>
</template>
<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
const isPlay = ref(false)
const refVideo = ref()

onMounted(() => {
	console.log()
	
})

function play() {
	isPlay.value = true
}

function playFn() {
	uni.createVideoContext('vedio').play()
}
</script>
<style lang="scss" scoped>
	.icon-back {
		position: absolute;
		top: 40rpx;
		left: 40rpx;
		z-index: 3;
		width: 50rpx;
		height: 50rpx;
	}
	.to-detial {
		position: absolute;
		width:200rpx;
		height: 200rpx;
		z-index: 3;
		bottom: 400rpx;
		left: 0;
	}
	.video-op {
		position: absolute;
		display: flex;
		flex-direction: column;
		right: 40rpx;
		bottom: 500rpx;
		image {
			width: 100rpx;
			height: 100rpx;
			margin: 40rpx 0;
		}
	}
	.video-detail {
		position: absolute;
		left: 30rpx;
		bottom: 80rpx;
		z-index: 3;
		color: #ffffff;
	}
	.play {
		width: 200rpx;
		width: 200rpx;
		position: absolute;
		top: 0;
		bottom: 0;
		right: 0;
		left: 0;
		margin:auto;
		z-index: 3;
	}
	video {
		position: absolute;
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.video-img {
		position: absolute;
		z-index: 2;
		width: 100%;
		top: 0;
		bottom: 0;
		right: 0;
		left: 0;
		margin:auto;
		transition: all 1s;
		&.active {
			width: 200rpx;
			height: 200px;
			bottom: 20rpx;
			right: 20rpx;
			margin:0;
			top: auto;
			left: auto;
		}
	}
</style>