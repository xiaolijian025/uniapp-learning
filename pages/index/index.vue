<template>
	
		<view  scroll-y="true">
			
				<!-- 轮播图组件 -->
				<swiper-image  :resdata="newsitems.siwper" />
				
				<!-- 首页分类 -->
				<index-nav :resdata="newsitems.indexnavs" />
				<divider />
				
				<divider />
				<!-- 大图广告位 -->
				<card headTitle="每日精选" bodyCover="/static/images/demo/demo4.jpg" />
				
				<!-- 公共列表组件 750 - 5 = 745   372.5-->
				<view class="row j-sb" >
					<block v-for="(item2,index2) in newsitems.goodlist" :key="index2">
						<common-list :item="item2" :index="index2" />
					</block>
				</view>

				<divider />
		</view>

</template>

<script>
	import swiperImage from "@/components/index/swiper-image.vue";
	import indexNav from "@/components/index/index-nav.vue";
	import threeAdv from "@/components/index/three-adv.vue";
	import card from "@/components/common/card.vue";
	import commonList from "@/components/common/common-list.vue";
	export default {
		components: {
			swiperImage,
			indexNav,
			threeAdv,
			card,
			commonList
		},
		data() {
			return {
				scrollinto:"",
				scrollH:500,
				tabIndex:0,
				tabBars: [],
				newsitems:{
					"siwper":[
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/5d8833ed4ef66.jpg",
							"id":"1"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/5d8833ed94392.png",
							"id":"2"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/5d8833ed94392.jpg",
							"id":"3"
						}
						
					],
					"indexnavs":[
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/1.png",
							"text":"新品发布"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/5.png",
							"text":"超值特卖"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/6.png",
							"text":"商城秒杀"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/7.png",
							"text":"真心想要"
						},
						{
							"src":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/10.png",
							"text":"其他选项"
						}
					],
					"goodlist":[
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/5984928facd569ef906becba3469810b.png",
							"title":"iphoneX",
							"desc":"全新十代酷睿处理器 / MX250独显 / OLED屏幕 / 3D面部识别 / 支持AirPower无线充电",
							"oprice":6699,
							"pprice":5399
						},
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/8a3b42e4956d8d00e40ddd4a7c470452.png",
							"title":"iphoneXR显示屏",
							"desc":"原装iphoneX十代显示屏 / OLED屏幕 / 3D面部识别 / 一个月保修反产",
							"oprice":699,
							"pprice":399
						},
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/765a18a017acf0b5c012ce765696be6f.png",
							"title":"iphoneXR无线蓝牙耳机",
							"desc":"原装iphoneX无线蓝牙耳机 / 防水 / 低音品质 / 炫酷多彩任你挑选",
							"oprice":1699,
							"pprice":1299
						},
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/575977783cdaf86f36c41df05bd9885b.png",
							"title":"荣耀meta P30",
							"desc":"全新十代酷睿处理器 / 14寸超窄边框高清屏 / 手环极速解锁 / 充电10分钟",
							"oprice":4699,
							"pprice":3399
						},
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/eb5a2d258905e0a6096af057946b0bf0.png",
							"title":"iphoneXR手机壳",
							"desc":"全皮iphoneXR手机壳 / 超软舒适 / 不沾灰 / 背景定制 / 炫酷多彩任你挑选",
							"oprice":99,
							"pprice":69
						},
						{
							"id":25,
							"cover":"https://tangzhe123-com.oss-cn-shenzhen.aliyuncs.com/public/e5cbf06f9e4c1387e81746b6bc2e3fbb.png",
							"title":"iphoneXR手机壳",
							"desc":"全皮iphoneXR手机壳 / 超软舒适 / 不沾灰 / 背景定制 / 炫酷多彩任你挑选",
							"oprice":99,
							"pprice":69
						},
					]
					
				}

			}
		},
		onLoad() {
			// 获取可视区域高度
			uni.getSystemInfo({
				success: (res) => {		
					this.scrollH = res.windowHeight - uni.upx2px(82)
					console.log(this.scrollH)
				}
			})
			// 初始化事件
			this.__init()
		},
		methods: {
			// 初始化事件
			__init(){
				// 获取顶部选项卡
				this.$H.get('/index_category/data').then((res)=>{
					
				})
			},
			
		}
	}
</script>

<style>

</style>
