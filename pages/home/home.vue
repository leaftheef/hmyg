<template>
	<view>
		<view class="topSearch">
			<view class="search_btn">
				搜索
			</view>
		</view>
		<view>
			<!-- 轮播图区域 -->
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
				<!-- 循环渲染轮播图的 item 项 -->
				<swiper-item v-for="(item, i) in banners" :key="i">
					<view class="swiper-item">
						<!-- 动态绑定图片的 src 属性 -->
						<image @click="goGoods_detail(item.goods_id)" :src="item.image_src"></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item, i) in navs" :key="i" @click="navClickHandler(item)">
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
		<!-- 楼层区域 -->
		<view class="floor-list">
			<!-- 楼层 item 项 -->
			<view class="floor-item" v-for="(item, i) in floors" :key="i">
				<!-- 楼层标题 -->
				<image :src="item.floor_title.image_src" class="floor-title"></image>
				<!-- 楼层图片区域 -->
				<view class="floor-img-box">
					<!-- 左侧大图片的盒子 -->
					<view class="left-img-box">
						<image class="left-img" :src="item.product_list[0].image_src"
							:style="{width: item.product_list[0].image_width + 'rpx'}"></image>
					</view>
					<!-- 右侧 4 个小图片的盒子 -->
					<view class="right-img-box">
						<view class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0">
							<image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}">
							</image>
						</view>
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	import {
		getBanners,
		getNavList,
		getFloorList
	} from '@/api/home.js'
	export default {
		data() {
			return {
				banners: [],
				navs: [],
				floors: []
			};
		},
		methods: {
			async loadBanners() {
				const res = await getBanners()
				this.banners = res.message
				// console.log(this.banners)
			},
			goGoods_detail(id) {
				uni.navigateTo({
					url: '/subpkg/goods-detail/goods-detail?goods_id=' + id
				})
			},
			// 加载导航列表数据
			async loadNavList() {
				const res = await getNavList()
				// console.log(res)
				this.navs = res.message
			},
			// nav-item 项被点击时候的事件处理函数
			navClickHandler(item) {
				// 判断点击的是哪个 nav
				if (item.name === '分类') {
					uni.switchTab({
						url: '/pages/cate/cate'
					})
				}
			},
			async getFloorList() {
				const res = await getFloorList()
				console.log(res)
				this.floors = res.message
			}
		},
		onLoad() {
			this.loadBanners()
			this.loadNavList()
			this.getFloorList()
		}
	}
</script>

<style lang="scss">
	.topSearch {
		width: 750rpx;
		height: 70px;
		background-color: #c00000;
		display: flex;
		justify-content: center;
		overflow: hidden;

		.search_btn {
			width: 400rpx;
			height: 30px;
			margin-top: 30px;
			background-color: #fff;
			border-radius: 10px;
			display: flex;
			justify-content: center;
			align-items: center;
		}
	}

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

		.nav-img {
			width: 128rpx;
			height: 140rpx;
		}
	}

	.floor-title {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.floor-img-box {
		display: flex;
		padding-left: 10rpx;

		.left-img {
			height: 392rpx !important;
		}
	}
</style>
