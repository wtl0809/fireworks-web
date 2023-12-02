<template>
	<view>
		<s-layout :onShareAppMessage="shareInfo" navbar="goods">
			<!-- 标题栏 -->
			<detailNavbar />

			<!-- 骨架屏 -->
			<detailSkeleton v-if="state.skeletonLoading" />
			<!-- 下架/售罄提醒 -->

			<block v-else>
				<view class="detail-swiper-selector">
					<!-- 商品轮播图  -->
					<su-swiper class="ss-m-b-14" isPreview :list="state.goodsSwiper" dotStyle="tag" imageMode="widthFix"
						dotCur="bg-mask-40" :seizeHeight="750" />

					<!-- 价格+标题 -->
					<view class="title-card detail-card ss-p-y-20 ss-p-x-20">
						<view class="title-text ss-line-2 ss-m-b-6">{{ state.goodsInfo.title }}</view>
						<view class="subtitle-text ss-line-1">{{ state.goodsInfo.subtitle }}</view>
					</view>
				</view>

				<!-- 详情 -->
				<view class="ss-p-20 ss-p-x-40 detail-card">
					<view class="detail-item">
						<view class="label">类别：</view> 类型一类型一类型一类型一类型一类型一类型一类型一类型一类型一类型一
					</view>
					<view class="detail-item">
						<view class="label">类别：</view> 类型一
					</view>
					<view class="detail-item">
						<view class="label">类别：</view> 类型一
					</view>
					<view class="detail-item">
						<view class="label">类别：</view> 类型一
					</view>
				</view>


				<!-- 详情tabbar -->
				<detail-tabbar v-model="state.goodsInfo">
					<view class="buy-box ss-flex ss-col-center ss-p-r-20" v-if="state.goodsInfo.stock > 0">
						<button class="ss-reset-button add-btn ui-Shadow-Main" @tap="state.showSelectSku = true">
							加入购物车
						</button>
						<button class="ss-reset-button buy-btn ui-Shadow-Main" @tap="state.showSelectSku = true">
							立即购买
						</button>
					</view>
					<view class="buy-box ss-flex ss-col-center ss-p-r-20" v-else>
						<button class="ss-reset-button disabled-btn" disabled> 已售罄 </button>
					</view>
				</detail-tabbar>

			</block>
		</s-layout>
	</view>
</template>

<script setup>
	import {
		reactive,
		computed
	} from 'vue';
	import {
		onLoad,
		onPageScroll
	} from '@dcloudio/uni-app';
	import sheep from '@/sheep';
	import {
		formatSales,
		formatGoodsSwiper,
		formatPrice
	} from '@/sheep/hooks/useGoods';
	import detailNavbar from './components/detail/detail-navbar.vue';
	import detailTabbar from './components/detail/detail-tabbar.vue';
	import detailSkeleton from './components/detail/detail-skeleton.vue';
	import {
		isEmpty
	} from 'lodash';
	onPageScroll(() => {});

	const state = reactive({
		goodsId: 0,
		skeletonLoading: true,
		goodsInfo: {},
		showSelectSku: false,
		goodsSwiper: [],
		selectedSkuPrice: {},
		showModel: false,
		total: 0,
		couponInfo: [],
		showActivityModel: false,
		activityInfo: [],
	});

	// 规格变更
	function onSkuChange(e) {
		state.selectedSkuPrice = e;
	}

	// 添加购物车
	function onAddCart(e) {
		sheep.$store('cart').add(e);
	}

	// 立即购买
	function onBuy(e) {
		sheep.$router.go('/pages/order/confirm', {
			data: JSON.stringify({
				order_type: 'goods',
				goods_list: [{
					goods_id: e.goods_id,
					goods_num: e.goods_num,
					goods_sku_price_id: e.id,
				}, ],
			}),
		});
	}
	//营销活动
	function onActivity() {
		state.activityInfo = state.goodsInfo.promos;
		state.showActivityModel = true;
	}



	const shareInfo = computed(() => {
		if (isEmpty(state.goodsInfo)) return {};
		return sheep.$platform.share.getShareInfo({
			title: state.goodsInfo.title,
			image: sheep.$url.cdn(state.goodsInfo.image),
			desc: state.goodsInfo.subtitle,
			params: {
				page: '2',
				query: state.goodsInfo.id,
			},
		}, {
			type: 'goods', // 商品海报
			title: state.goodsInfo.title, // 商品标题
			image: sheep.$url.cdn(state.goodsInfo.image), // 商品主图
			price: state.goodsInfo.price[0], // 商品价格
			original_price: state.goodsInfo.original_price, // 商品原价
		}, );
	});

	onLoad(async (options) => {
		// 非法参数
		if (!options.id) {
			state.goodsInfo = null;
			return;
		}
		state.goodsId = options.id;
		// 加载商品信息
		sheep.$api.goods.detail(state.goodsId).then((res) => {
			state.skeletonLoading = false;
			if (res.code === 1) {
				state.goodsInfo = res.data;
				state.goodsSwiper = formatGoodsSwiper(state.goodsInfo.images);
			} else {
				// 未找到商品
				state.goodsInfo = null;
			}
		});
		const {
			code,
			data
		} = await sheep.$api.coupon.listByGoods(state.goodsId);
		if (code === 1) {
			state.couponInfo = data;
		}
	});
</script>

<style lang="scss" scoped>
	.detail-item {
		margin: 18rpx 0;
		display: flex;
		align-items: center;
		.label {
			$width: 50px;
			color: #999;
			width: $width;
			min-width: $width;
		}

		&:before {
			$width: 16rpx;
			content: '';
			display: block;
			width: $width;
			height: $width;
			min-width: $width;
			min-height: $width;
			background: linear-gradient(to right, rgb(255, 96, 0), rgb(254, 131, 42));
			border-radius: $width;
			margin-right: $width;
		}
	}

	.detail-card {
		background-color: #ffff;
		margin: 14rpx 20rpx;
		border-radius: 10rpx;
		overflow: hidden;
	}

	// 价格标题卡片
	.title-card {
		.price-box {
			.price-text {
				font-size: 42rpx;
				font-weight: 500;
				color: #ff3000;
				line-height: 30rpx;
				font-family: OPPOSANS;

				&::before {
					content: '￥';
					font-size: 30rpx;
				}
			}

			.origin-price-text {
				font-size: 26rpx;
				font-weight: 400;
				text-decoration: line-through;
				color: $gray-c;
				font-family: OPPOSANS;

				&::before {
					content: '￥';
				}
			}
		}

		.sales-text {
			font-size: 26rpx;
			font-weight: 500;
			color: $gray-c;
		}

		.discounts-box {
			.tag-content {
				flex: 1;
				min-width: 0;
				white-space: nowrap;
			}

			.tag-box {
				overflow: hidden;
				text-overflow: ellipsis;
			}

			.tag {
				flex-shrink: 0;
				padding: 4rpx 10rpx;
				font-size: 24rpx;
				font-weight: 500;
				border-radius: 4rpx;
				color: var(--ui-BG-Main);
				background: var(--ui-BG-Main-tag);
			}

			.discounts-title {
				font-size: 24rpx;
				font-weight: 500;
				color: var(--ui-BG-Main);
				line-height: normal;
			}

			.cicon-forward {
				color: var(--ui-BG-Main);
				font-size: 24rpx;
				line-height: normal;
				margin-top: 4rpx;
			}
		}

		.title-text {
			font-size: 30rpx;
			font-weight: bold;
			line-height: 42rpx;
		}

		.subtitle-text {
			font-size: 26rpx;
			font-weight: 400;
			color: $dark-9;
			line-height: 42rpx;
		}
	}

	// 购买
	.buy-box {
		.add-btn {
			width: 214rpx;
			height: 72rpx;
			font-weight: 500;
			font-size: 28rpx;
			border-radius: 40rpx 0 0 40rpx;
			background-color: var(--ui-BG-Main-light);
			color: var(--ui-BG-Main);
		}

		.buy-btn {
			width: 214rpx;
			height: 72rpx;
			font-weight: 500;
			font-size: 28rpx;

			border-radius: 0 40rpx 40rpx 0;
			background: linear-gradient(90deg, var(--ui-BG-Main), var(--ui-BG-Main-gradient));
			color: $white;
		}

		.disabled-btn {
			width: 428rpx;
			height: 72rpx;
			font-weight: 500;
			font-size: 28rpx;

			border-radius: 0 40rpx 40rpx 0;
			background: linear-gradient(90deg, var(--ui-BG-Main), var(--ui-BG-Main-gradient));
			color: $white;
		}
	}

	.model-box {
		height: 60vh;

		.model-content {
			height: 56vh;
		}

		.title {
			font-size: 36rpx;
			font-weight: bold;
			color: #333333;
		}

		.subtitle {
			font-size: 26rpx;
			font-weight: 500;
			color: #333333;
		}
	}
</style>