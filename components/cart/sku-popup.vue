<template>
	<common-popup :popupClass="popupShow" @hide="doHidePopup">
		<view class="d-flex a-center" style="height: 275rpx;">
			<image src="../../static/images/demo/list/1.jpg" mode="widthFix"
			style="height: 180rpx;width: 180rpx;" class="border rounded"></image>
			<view class="pl-2">
				<price priceSize="font-lg" unitSize="font">2365</price>
				<view class="d-block">
					<text class="mr-1"
					v-for="(attr,attrIndex) in popupData.attrs"
					:key="attrIndex">{{attr.list[attr.selected].name}}</text>
				</view>
			</view>
		</view>
		<scroll-view scroll-y class="w-100" style="height: 660rpx;">
			<card :headTitle="item.title" :headTitleWeight="false" 
			:headBorderBottom="false" :key="index"
			v-for="(item,index) in popupData.attrs">
				<zcm-radio-group :label="item" 
				:selected.sync='item.selected'></zcm-radio-group>
			</card>
			<view class="d-flex j-sb a-center p-2 border-top border-light-secondary">
				<text>购买数量</text>
				<uni-number-box :min="1" :value="popupData.num" @change="popupData.num = $event"></uni-number-box>
			</view>
		</scroll-view>
		 <view class="main-bg-color text-white font-md d-flex a-center j-center" hover-class="main-bg-hover-color" style="height: 100rpx;margin-left: -30rpx;margin-right: -30rpx;" @tap.stop="doHidePopup">
		 	确定
		 </view>
	</common-popup>
</template>

<script>
	import commonPopup from "@/components/common/common-popup.vue"
	import price from "@/components/common/price.vue"
	import zcmRadioGroup from "@/components/common/radio-group.vue"
	import card from "@/components/common/card.vue"
	import uniNumberBox from "@/components/uni-ui/uni-number-box/uni-number-box.vue"
	import {mapState,mapGetters,mapActions,mapMutations} from "vuex"
	export default {
		components: {
			commonPopup,
			price,
			zcmRadioGroup,
			card,
			uniNumberBox
		},
		computed: {
			...mapState({
				popupShow:state=>state.cart.popupShow
			}),
			...mapGetters(['popupData'])
		},
		methods: {
			...mapActions([
				'doHidePopup',
			])
		},
	}
</script>

<style>
</style>
