<template>
	<view class="p-2">
		<view class="font-lg mb-2">{{detail.title}}</view>
		
		<price priceSize="font-lg" unitSize="font">
		{{showPrice}}</price>
		
		<!-- <view class="font text-light-muted mb-3 line-h-sm">
			{{detail.desc}}
		</view> -->
		
		<view class="bot-row d-flex a-center font-md text-light-muted mt-2" style="height: 50rpx;">
			<text>销量: 108</text>
			<text>库存: 4690</text>
			<text>浏览量: 768</text>
		</view>
	</view>
</template>

<script>
	import price from "@/components/common/price.vue"
	export default {
		components: {
			price
		},
		props: ['detail','showPrice'],
	}
</script>

<style scoped>
	.bot-row text {
		flex: 1;
		
	}
</style>
