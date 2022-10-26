<template>
	<view class="list-box">
		<view class="left-box">
			<image :src="goods.goods_small_logo || defaultPic" class="goods-pic"></image>
		</view>
	
		<view class="right-box">
			<view class="goods-name">
				{{goods.goods_name}}
			</view>
			<view class="goods-price">
				￥{{goods.goods_price | tofixed}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-goods",
		// 定义 props 属性，用来接收外界传递到当前组件的数据
		    props: {
		      // 商品的信息对象
		      goods: {
		        type: Object,
		        defaul: {},
		      },
		    },
		data() {
			return {
				defaultPic: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.51qianduan.com%2Fpublic%2Fimages%2Ffooter_nopic.jpg&refer=http%3A%2F%2Fwww.51qianduan.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1669347305&t=800dc5035c0363f9742470d96014f197'
			};
		},
		filters:{
			tofixed(num){
				return	Number(num).toFixed(2)
			}
		}
	}
</script>

<style lang="scss">
.list-box {
		display: flex;
		padding: 10px 5px;
		border-bottom: 1px solid #f0f0f0;

		.left-box {
			margin-right: 5px;

			.goods-pic {
				width: 100px;
				height: 100px;
				display: block;
			}
		}

		.right-box {
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.goods-name {
				font-size: 13px;
			}

			.goods-price {
				font-size: 16px;
				color: #c00000;
			}
		}

	}
</style>