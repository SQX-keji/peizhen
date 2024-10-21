<template>
	<view style="padding-bottom: 200rpx;">
		<!-- 车辆列表 -->
		<view class="carList">
			<view class="carList-box">
				<view class="carList-box-item" v-for="(item,index) in dataList" :key="index" @click.stop="getcar(item)">
					<view class="carList-box-item-r">
						<view class="carList-box-item-r-name">{{item.realName}} {{item.phone}}</view>
						<view class="margin-tb-xs">{{item.idNumber}}</view>
						<view class="carList-box-item-r-label">
							<view v-if="item.sex==1">男</view>
							<view v-if="item.sex==2">女</view>
							<view class="padding-lr-xs">|</view>
							<view v-if="item.isUnderAge==1">已满十八岁</view>
							<view v-if="item.isUnderAge==2">未满十八岁</view>
							<view class="padding-lr-xs">|</view>
							<view>{{item.relationship}}</view>
						</view>
						<view class="margin-tb-xs" style="color: #999999;" v-if="item.emergencyPhone">紧急联系人：{{item.emergencyPhone}}</view>
					</view>
					
					<view class="flex align-end justify-end margin-right-sm ">
						<view class="flex align-center text-right">
							<view @click.stop="bianji(item.patientId)">
								<image src="../static/bianji.png" style="width: 35upx;height: 35upx;"></image>
							</view>
							<view class="margin-left" @click.stop="dete(item.patientId)">
								<image src="../static/dete.png" style="width: 35upx;height: 38upx;"></image>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 添加车辆 -->
		<view class="addCar">
			<view class="addCar-box" @click="gotoAddCar()">
				添加就诊人
			</view>
		</view>
		<empty v-if="dataList.length == 0"></empty>
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
				dataList: [],
				page: 1,
				limit: 10,
				type: ''
			};
		},
		onLoad(option) {
			uni.showLoading({
				title: '加载中....'
			})
			if (option.type) {
				this.type = option.type
			}
		},
		onShow() {
			this.getcarlist()
		},
		methods: {
			getcar(e) {
				console.log(this.type)
				if (this.type == 1) {
					// let obj = {
					// 	realName:e.realName,
					// 	patientId:e.patientId
					// }
					uni.$emit('updateData', e)
					uni.navigateBack()
				}else if(this.type !=4){
					uni.$emit('jiuzhenlist', e)
					// uni.setStorageSync("jiuzhenlist", e)
					uni.navigateBack()
				}
			},
			//添加车辆
			gotoAddCar() {
				uni.navigateTo({
					url: './addCar'
				})
			},
			//重新编辑
			bianji(patientId) {
				uni.navigateTo({
					url: '/my/other/addCar?patientId=' + patientId
				})
			},
			//删除
			dete(patientId) {
				uni.showLoading({
					title: '正在删除'
				})
				let data = {
					patientId: patientId
				}
				this.$Request.getT("/app/patientInfo/deletePatient", data).then(res => {
					uni.hideLoading();
					if (res.code == 0) {
						uni.showToast({
							title: '删除成功',
							icon: 'none'
						})
						this.page = 1;
						this.getcarlist()
					} else {
						console.log(res.msg)
					}
				})
			},
			getcarlist() {

				let data = {
					page: this.page,
					limit: this.limit
				}
				this.$Request.get("/app/patientInfo/getPatientList", data).then(res => {
					uni.hideLoading();
					if (res.code == 0) {
						if (this.page == 1) {
							this.dataList = res.data.records
						} else {
							this.dataList = [...this.dataList, ...res.data.records]
						}
					} else {
						console.log(res.msg)
					}
					uni.stopPullDownRefresh();
				})
			}
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getcarlist()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getcarlist()
		},
	}
</script>

<style lang="less">
	page {
		background: #F5F5F5;
	}

	.carList {
		width: 100%;
		height: auto;
		display: flex;
		justify-content: center;
		align-items: center;


		.carList-box {
			width: 686rpx;
			height: auto;

			.carList-box-item {
				width: 100%;
				background-color: #FFFFFF;
				border-radius: 16rpx;
				margin-top: 20rpx;
				padding: 20upx;
			}

			.carList-box-item-r {
				margin-left: 17rpx;

				.carList-box-item-r-name {
					color: #1E1F31;
					font-size: 28rpx;
					font-weight: bold;
				}

				.carList-box-item-r-label {
					color: #999999;
					font-size: 24rpx;
					margin-top: 10rpx;
					display: flex;
					align-items: center;

				}
			}
		}
	}


	.addCar {
		width: 100%;
		height: 150rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		position: fixed;
		bottom: 0;
		background-color: #F5F5F5;
		z-index: 999;

		.addCar-box {
			width: 686rpx;
			height: 88rpx;
			border-radius: 44rpx;
			background-color: #0175FE;
			color: #FFFFFF;
			font-size: 28rpx;
			font-weight: bold;
			display: flex;
			justify-content: center;
			align-items: center;
		}
	}
</style>
