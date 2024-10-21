<template>
	<view>
		<view class="header flex justify-center">
			<view class="header-box flex align-center">
				<view class="header-box-l">
					<image :src="hospitalInfo.hospitalImg" mode=""></image>
				</view>
				<view class="header-box-r">
					<view class="header-box-r-t">
						{{hospitalInfo.hospitalName}}
					</view>
					<view class="header-box-r-b">
						<text>{{hospitalInfo.hospitalLevel}}</text>
						<text>{{hospitalInfo.hospitalType}}</text>
					</view>
				</view>
			</view>
		</view>
		<!-- 重点科室 -->
		<view class="ks flex justify-center">
			<view class="ks-box">
				<text>重点科室：</text>
				<text>
					{{hospitalInfo.departmentDetails}}
				</text>
			</view>
		</view>
		<view class="jianjie flex justify-center">
			<view class="jianjie-box">
				<view class="jianjie-box-title">
					医院简介
				</view>
				<view class="jianjie-box-in">
					{{hospitalInfo.hospitalDetails}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				hospitalId: '',
				hospitalInfo: {}
			};
		},
		onLoad(option) {
			this.hospitalId = option.hospitalId
			this.getyiyuanInfo()
		},
		methods: {
			//医院详情
			getyiyuanInfo() {
				let data = {
					hospitalId: this.hospitalId
				}
				this.$Request.getT('/app/hospital/getHospitalInfo', data).then(res => {
					if (res.code == 0) {
						this.hospitalInfo = res.data
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'error'
						})
					}
				});
			},
		}
	}
</script>

<style lang="scss">
	.header{
		width: 100%;
		height: 200rpx;
		.header-box{
			width: 686rpx;
			height: 100%;
			border-radius: 16rpx;
			// background-color: red;
			.header-box-l{
				width: 240rpx;
				height: 130rpx;
				image{
					width: 100%;
					height: 100%;
					border-radius: 16rpx;
				}
			}
			.header-box-r{
				margin-left: 30rpx;
				.header-box-r-t{
					font-size: 38rpx;
					font-weight: bold;
				}
				.header-box-r-b{
					margin-top: 20rpx;
					text:nth-of-type(1){
						color: red;
					}
					text:nth-of-type(2){
						color: #999999;
						margin-left: 20rpx;
					}
				}
			}
		}
	}
	.ks{
		width: 100%;
		margin-top: 20rpx;
		.ks-box{
			width: 686rpx;
			height: 100%;
			font-size: 26rpx;
			text:nth-of-type(1){
				font-weight: bold;
			}
			text:nth-of-type(2){
				color: #999999;
				letter-spacing: 3px;
			}
		}
	}
	.jianjie{
		width: 100%;
		height: auto;
		margin-top: 30rpx;
		.jianjie-box{
			width: 686rpx;
			font-size: 26rpx;
			.jianjie-box-title{
				font-weight: bold;
			}
			.jianjie-box-in{
				margin-top: 20rpx;
				color: #999999;
				letter-spacing: 3px;
			}
		}
	}


</style>
