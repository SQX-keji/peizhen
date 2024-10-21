<template>
	<view>
		<view class="info flex justify-center align-center">
			<view class="info-box">
				<view class="info-box-avatar flex ">
					<image :src="userInfo.avatar?userInfo.avatar:'../../static/logo.png'" mode=""></image>
					<view class="info-box-info">
						<view class="info-box-info-name flex align-center">
							{{userInfo.name}}
							<image v-if="userInfo.userEntity && userInfo.userEntity.iconImg" style="width: 30rpx;height: 30rpx;margin-left: 20rpx;border-radius: 0;" :src="userInfo.userEntity.iconImg" mode="widthFix"></image>
							<text v-if="userInfo.age" style="margin-left: 10rpx;font-weight: 400;font-size: 28rpx;">{{userInfo.age}}岁</text>
							<u-icon v-if="userInfo.sex===1" style="margin-left: 10rpx;" name="man" color="#ffffff" size="32"></u-icon>
							<u-icon v-else style="margin-left: 10rpx;" name="woman" color="pink" size="32"></u-icon>
						</view>
						<view class="info-box-info-address">
							{{userInfo.province?userInfo.province:''}} {{userInfo.city?userInfo.city:''}}
						</view>
						<view class="info-box-info-orderNum">
							接单量：{{userInfo.userEntity && userInfo.userEntity.orderCount?userInfo.userEntity.orderCount:0}}单
						</view>
						<view class="info-box-info-fuwu flex align-center">
							服务评价：
							<u-rate :count="count" v-model="value" active-color="#FFAA01"
								inactive-color="#FFAA01"
								style="margin-left: -5rpx;"></u-rate>{{userInfo.userEntity && userInfo.userEntity.finalScore?userInfo.userEntity.finalScore:'5.0'}}
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 服务 -->
		<view class="fuwu flex justify-center align-center">
			<view class="fuwu-box">
				<view class="fuwu-box-title">
					服务项目
				</view>
				<view class="fuwu-box-fuwu">
					{{userInfo.serviceName?userInfo.serviceName:'暂无'}}
				</view>
				<view class="fuwu-box-title">
					自我介绍
				</view>
				<view class="fuwu-box-jieshao">
					{{userInfo.details?userInfo.details:'暂无'}}
				</view>
			</view>
		</view>
		<view class="changeTa flex justify-center align-center" @click="goBack(userInfo)">
			立即下单
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				count:1,
				value:1,
				userId:'',
				userInfo:{},
				back:false,
				typeInfo:{}
			};
		},
		onLoad(e) {
			this.userId = e.userId
			if(e.back){
				this.back = e.back
			}else{
				this.back = false
			}
			if(e.info){
				this.typeInfo = JSON.parse(decodeURIComponent(e.info))
			}else{
				this.typeInfo = {}
			}
			this.getUserInfo()
		},
		methods:{
			getUserInfo(){
				let data = {
					userId:this.userId
				}
				this.$Request.getT('/app/userCertification/getNursingInfo',data).then(res=>{
					if(res.code == 0){
						this.userInfo = res.data
					}else{
						uni.showToast({
							title:res.msg,
							icon:'none'
						})
					}
				})
			},
			// 携带参数跳转
			goBack(item) {
				if(!uni.getStorageSync('token')){
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}else{
					if (item.userEntity.workStatus == 0) { //不忙碌
						// let objInfo = {
						// 	orderTakingUserName: item.realName,
						// 	orderTakingUserId: item.userId
						// }
						if(JSON.stringify(this.typeInfo) == '{}'){
							if(this.back){
								let objInfo = {
									orderTakingUserName: item.name,
									orderTakingUserId: item.userId
								}
								uni.$emit('peizhenPeople',objInfo)
								uni.navigateBack({
									delta:2
								})
							}else{
								uni.navigateTo({
									url:'/my/peizhen/peizhen?modularId=1&orderTakingUserName='+item.name+'&orderTakingUserId='+item.userId
								})
							}
						}else{
							uni.navigateTo({
								url:'/my/peihu/order?hguserId='+item.userId+'&info='+encodeURIComponent(JSON.stringify(this.typeInfo))
							})
						}
						
						
					} else {
						uni.showToast({
							title: '请选择空闲状态的护理员',
							icon: 'none'
						})
					}
				}
				
				// uni.navigateBack()
			},
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #ffffff;
	}
	// .468EF8
	.info{
		width: 100%;
		height: 300rpx;
		background-color: #468EF8;
		.info-box{
			width: 686rpx;
			height: 100%;
			// background-color: red;
		}
		.info-box-avatar{
			width: 100%;
			image{
				width: 150rpx;
				height: 150rpx;
				border-radius: 50%;
			}
			.info-box-info{
				margin-left: 20rpx;
				color: #ffffff;
				.info-box-info-name{
					font-size: 40rpx;
					font-weight: bold;
					
				}
				.info-box-info-address{
					margin-top: 10rpx;
				}
				.info-box-info-orderNum{
					margin-top: 10rpx;
				}
				.info-box-info-fuwu{
					margin-top: 10rpx;
				}
			}
		}
	}

	.fuwu{
		width: 100%;
		height: auto;
		background-color: #ffffff;
		border-radius: 40rpx 40rpx 0 0;
		margin-top: -80rpx;
		.fuwu-box{
			width: 686rpx;
			height: 100%;
		}
		.fuwu-box-title{
			margin-top: 20rpx;
			font-size: 36rpx;
		}
		.fuwu-box-fuwu{
			background-color: #F5F5F5;
			padding: 20rpx;
			border-radius: 20rpx;
			margin-top: 10rpx;
		}
		.fuwu-box-jieshao{
			// padding: 20rpx;
			margin-top: 10rpx;
		}
	}
	.changeTa{
		width: 686rpx;
		height: 80rpx;
		position: fixed;
		bottom: 100rpx;
		left: 50%;
		transform: translate(-50%,0%);
		z-index: 99;
		border-radius: 40rpx;
		color: #ffffff;
		background-color: #468EF8;
	}
</style>