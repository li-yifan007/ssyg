<template>
	<view>
		<view class="searce-box">
			<my-search @click="gotoSearch"></my-search>
		</view>
		<!-- 轮播图 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item,i) in swiperList" :key="i">
				<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
					<image :src="item.image_src"></image>
				</navigator>
			</swiper-item>+
			
		</swiper>

		<!-- 分类导航 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item,i) in navList" :key="i" @click="navClickHandler(item)">
				<image class="nav-img" :src="item.image_src"></image>
			</view>
		</view> 
 
		<!-- 楼层部分 -->
		<view class="floor-list">
			<view class="floor-item" v-for="(item,i) in floorList" :key="i">
				<image :src="item.floor_title.image_src" class="floor-list-img"></image>
				<!-- 楼层图片区 -->
				<view class="floor-img">
					<navigator class="floor-left-img" :url="item.product_list[0].url">
						<image :src="item.product_list[0].image_src"
							:style="{width: item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
					</navigator>
					<view class="floor-right-box">
						<navigator class="floor-right-img" :url="item2.url" v-for="(item2,i2) in item.product_list"
							:key="i2">
							<image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"
								v-if="i2 !== 0"></image>
						</navigator>
					</view>
				</view>
			</view>
		</view>


	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabbar-badge.js'
	
	export default {
		mixins: [badgeMix],
		data() {
			return {
				swiperList: [],
				navList: [],
				floorList: []
			};
		},
		onLoad() {
			this.getSwiperList()
			this.getNavList()
			this.getFloorList()
		},
		methods: {
			async getSwiperList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata')
				// console.log(res)
				if (res.meta.status !== 200) return uni.$showMsg()
				this.swiperList = res.message
				// uni.$showMsg('666666')
			},
			async getNavList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200) return uni.$showMsg()
				this.navList = res.message
			},
			async getFloorList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/floordata')
				// console.log(res);	
				if (res.meta.status !== 200) return uni.$showMsg()
				// 双层forEach循环,处理URL地址
				res.message.forEach(floor => {
					floor.product_list.forEach(prod => {
						prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
					})
				})
				this.floorList = res.message
			},
			gotoSearch() {
				uni.navigateTo({
					url: '/subpkg/search/search'
				})
			},
			navClickHandler(item) {
				// console.log(item);
				if (item.name === '分类') {
					uni.switchTab({
						url: '/pages/cate/cate'
					})
				}
			}
		}
	}
</script>

<style lang="scss">
	swiper {
		height: 330rpx;

		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}

	.nav-list {
		display: flex;
		justify-content: space-around;
		margin: 15px 0;
	}

	.nav-img {
		width: 128rpx;
		height: 140rpx;
	}

	.floor-list-img {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.floor-img {
		display: flex;
		padding-left: 10rpx;
	}

	.floor-right-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.searce-box {
		position: sticky;
		top: 0;
		z-index: 999;
	}
</style>
