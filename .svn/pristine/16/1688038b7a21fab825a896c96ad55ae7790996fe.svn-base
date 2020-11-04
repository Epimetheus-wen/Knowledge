<template>
	<view class="dot-main">
		<view :class="['dot-item',current==index?'active':'']" v-for="(item,index) in list" :key="index"></view>
	</view>
</template>
<style>
	.dot-main {
		width: 100vw;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.dot-item {
		width: 70rpx;
		height: 6rpx;
		/* border: 1rpx solid #000000; */
		margin: 0 7rpx;
		background: #C1C1CE;
		box-sizing: border-box;
	}
	.active {
		background: #2F7AF5; 
	}
</style>
<script>
	export default {
		props: [ 'list','current'],
		data() {
			return {
				
			};
		}
	}
</script>
