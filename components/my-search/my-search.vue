<template>
		<view class="my-search-container":style="{'background-color': bgcolor}" @click="searchBoxHandler">
			<view class="my-search-box" :style="{'border-radius': radius + 'px'}">
				<uni-icons type="search" size="18"></uni-icons>
				<text class="placeholder">搜索</text>
			</view>
		</view>
</template>

<script>
	export default {
		name:"my-search",
		data() {
			return {
				
			};
		},
		methods:{
			searchBoxHandler(){
				 this.$emit('click')
			}
		},
		props: {
		  // 背景颜色
		  bgcolor: {
		    type: String,
		    default: '#C00000'
		  },
		  // 圆角尺寸
		  radius: {
		    type: Number,
		    // 单位是 px
		    default: 18
		  }
		}
	}
</script>

<style lang="scss">
.my-search-container {
  // background-color: #c00000;
  height: 50px;
  padding: 0 10px;
  display: flex;
  align-items: center;
}

.my-search-box {
  height: 36px;
  background-color: #ffffff;
  // border-radius: 15px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  // position: sticky;
  // top: 0;
  // z-index: 999;

  .placeholder {
    font-size: 15px;
    margin-left: 5px;
  }
}
</style>