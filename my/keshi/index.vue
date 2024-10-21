<template>
	<view class="content">
		<!-- header -->
		<view class="header">
			<view class="header-box">
				<u-search placeholder="搜索你需要的科室" @search="search" :show-action="false" shape="square" v-model="keyword"></u-search>
			</view>
		</view>
		<!-- #ifdef H5 -->
		<view class="menu" style="top: 190rpx;">
		<!-- #endif -->
		<!-- #ifndef H5 -->
		<view class="menu" style="top: 90rpx;">
		<!-- #endif -->
			<!-- 左侧菜单 -->
			<view class="menuLeft">
				<scroll-view scroll-y="true" class="menuLeft-scroll">
					<view :class="current==index?'active':''" @click="qeihuan(index,item)" class="menuLeft-scroll-item"
						v-for="(item,index) in tabsList" :key="index">
						{{item.departmentName}}
					</view>
				</scroll-view>
			</view>
			<!-- 右侧内容 -->
			<view class="menuRight">
				<scroll-view scroll-y="true" class="menuRight-scroll">
					<view class="menuRight-scroll-item">
						<view class="menuRight-scroll-item-box" v-for="(item,index) in tabsListRight"
							@click="goDetai(item)" :key="index">
							{{item.departmentName}}
						</view>
					</view>
					<empty v-if="tabsListRight.length == 0"></empty>
				</scroll-view>
			</view>
		</view>

	</view>
</template>

<script>
	import empty from '../../components/empty.vue'
	export default {
		components: {
			empty
		},
		data() {
			return {
				keyword:'',
				current: 0,
				isVip: false,
				tabsList: [],
				tabsListRight: [],
				idHome: '',
				token: '',
				page: 1,
				limit: 10,
				hospitalId:'',
				userId: '',
			}
		},
		onLoad(option) {
			this.hospitalId = option.hospitalId
			if(this.hospitalId){
				this.getClassfly();
			}
		},
		onShow() {
			
		},
		methods: {
			//返回
			goDetai(item){
				let data = {
					departmentName:item.departmentName,
					departmentId:item.departmentId
				}
				uni.$emit('xzks',data)
				uni.navigateBack()
				// console.log(item)
			},
			//搜索
			search(){
				this.getClassfly()
			},
			// 获取类型
			getClassfly() {
				let _this = this
				let data = {
					hospitalId:this.hospitalId,
					departmentName:this.keyword
				}
				this.$Request.get("/app/department/getDepartmentList",data).then(res => {
					if (res.code == 0) {
						// console.log(res.data, '11111111111111111')
						if(res.data && res.data.length>0){
							_this.tabsList = res.data
							_this.tabsListRight = _this.tabsList[0].departmentsList
						}else{
							_this.tabsList = []
							_this.tabsListRight = []
						}
						
					}
				});
			},
			// 左侧筛选
			qeihuan(index, item) {
				this.current = index
				this.tabsListRight = this.tabsList[index].departmentsList
			},

		}
	}
</script>

<style lang="scss" scoped>
	.content {
		width: 100%;
		// height: calc(100vh - 88rpx);
		height: 100vh;
		background-color: #FFFFFF;
	}

	.header {
		width: 100%;
		height: 88rpx;
		background-color: #ffffff;
		display: flex;
		justify-content: center;
		align-items: center;
		position: fixed;
		/* #ifdef H5 */
		top: 80rpx;
		/* #endif */
		/* #ifndef H5 */
		top: 0;
		/* #endif */
		z-index: 99;

		.header-box {
			width: 686rpx;
			height: 60rpx;

			.header-box-search {
				width: 100%;
				height: 100%;
				background-color: rgb(247, 247, 247);
				border-radius: 10rpx;
				display: flex;
				align-items: center;
				padding-left: 30rpx;

				text {
					margin-left: 20rpx;
					font-size: 24rpx;
					color: #666666;
				}
			}
		}
	}

	.active {
		font-weight: bold !important;
		background-color: #F7F7F7;
		color: #557EFD !important;
		font-family: PingFang-SC-Bold !important;
		font-size: 32upx !important;
		border-left: 10rpx solid #557EFD;
		box-sizing: border-box;
		background-color: #ffffff;
	}

	.menu {
		width: 100%;
		height: calc(100vh - 88rpx);
		display: flex;
		// background-color: red;
		position: fixed;
		.menuLeft {
			// width: 220rpx;
			width: 35%;
			height: 100%;

			.menuLeft-scroll {
				width: 100%;
				height: 100%;
				background-color: #f3f4f6;
			}

			.menuLeft-scroll-item {
				width: 100%;
				height: 92rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				color: #333333;
				font-size: 28rpx;
				font-weight: 500;
			}
		}

		.menuRight {
			// width: 530rpx;
			width: 65%;
			height: 100%;
			position: relative;

			// background-color: yellow;
			.menuRight-search {
				width: 100%;
				height: 92rpx;
				position: absolute;
				top: 0;
			}

			.menuRight-scroll {
				width: 100%;
				height: calc(100% - 92rpx);
				position: absolute;
				top: 0rpx;

				.menuRight-scroll-item {
					width: 100%;

					.menuRight-scroll-item-box {
						width: 90%;
						height: 100%;
						background-color: #ffffff;
						border-radius: 8rpx;
						padding:25rpx 0;
						text-align: center;

					}
				}
			}
		}
	}

	.filter-wrapper {
		width: auto !important;
	}

	.navs {
		background-color: #F7F7F7 !important;
	}
</style>
