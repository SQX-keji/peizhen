<template>
	<view style="padding-bottom: 200rpx;">
		<!-- 背景图 -->
		<view class="bgImg">
			<image :src="info.backgroundImg" style="width: 100%;height: 480rpx;position: absolute;top: 0;left: 0;"
				mode=""></image>
			<view class="bgImg-box">
				<view class="bgImg-box-title">
					{{info.serviceName}}
				</view>
				<view class="bgImg-box-labs flex align-center">
					<image src="../../static/images/ico.png" style="width: 56rpx;height: 34rpx;margin-right: 10rpx;"
						mode=""></image>
					<view class="" style="width: 90%;">
						{{info.title}}
					</view>

				</view>
			</view>
			<view class="info flex justify-center">
				<view class="info-box flex justify-center">
					<view class="info-box-c">
						<view class="info-box-c-t flex align-center">
							{{info.serviceDescribe}}
						</view>
						<view class="info-box-c-b flex align-center">
							{{info.money}}<text>元/{{info.company==1?'天':'次'}}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="tabs flex justify-center">
			<view class="tabs-box flex align-center">
				<view :class="{active:current==0}" class="tabs-box-l" @click="tabs(0)">
					服务内容
				</view>
				<view :class="{active:current==1}" class="tabs-box-r" @click="tabs(1)">
					服务须知
				</view>
			</view>
		</view>
		<!-- 内容 -->
		<view class="cont flex justify-center">
			<view class="cont-box flex justify-center">
				<view class="cont-box-c" v-html="current==0?content:content2">

				</view>
			</view>
		</view>


		<!-- 立即预约 -->
		<view class="btn flex justify-center align-center" v-if="type==''">
			<view class="btn-box flex justify-center align-center" @tap="goNav(info)">
				立即预约￥{{info.money}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				info: {},
				current: 0,
				content: '',
				content2: '',
				type: '',
				tuiName: '',
				tuiImage: '',
				invitationCode: '',
				modularId: -1
			};
		},
		onShareAppMessage(res) {
			return {
				path: '/my/yxpeizhen/info?invitation=' + this
					.invitationCode + '&data=' + encodeURIComponent(JSON.stringify(this
						.info)), //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		/*
		 * uniapp微信小程序分享页面到微信朋友圈
		 */
		onShareTimeline(res) {
			return {
				path: '/my/yxpeizhen/info?invitation=' + this.invitationCode + '&data=' + encodeURIComponent(JSON
					.stringify(this.info)), //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		onLoad(option) {
			let that = this
			// 分享
			this.myId = uni.getStorageSync('userId')
			// 获取邀请码保存到本地
			if (option.invitation) {
				that.$queue.setData('inviterCode', option.invitation);
			}
			// #ifdef MP-WEIXIN
			if (option.scene) {
				const scene = decodeURIComponent(option.scene);
				that.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
			if (this.myId) {
				that.invitationCode = uni.getStorageSync('invitationCode')
				that.$Request.getT('/app/common/type/276').then(res => { //分享标题 276
					if (res.code == 0) {
						that.tuiName = res.data.value
					}
				})
				that.$Request.getT('/app/common/type/277').then(res => { //分享图 277
					if (res.code == 0) {
						that.tuiImage = res.data.value
					}
				})
			}
			this.info = JSON.parse(decodeURIComponent(option.data))
			if (option.modularId) {
				this.modularId = Number(option.modularId)
			} else {
				this.modularId = -1
			}
			this.content = this.info.serviceInstruction.replace("img",
				'img style="width:100%;height:auto"');
			this.content2 = this.info.serviceContent.replace("img",
				'img style="width:100%;height:auto"');
			if (option.type) {
				this.type = option.type
			}
		},
		methods: {
			tabs(num) {
				this.current = num
			},
			goNav(e) {
				if (uni.getStorageSync('token')) {
					if (this.modularId == -1) { //如果没有modularId，那么走原来的分支
						uni.navigateTo({
							url: '/pages/index/game/orderDet?serviceId=' + e.serviceId + '&hospitalName=' + e
								.hospitalName + '&hospitalId=' + e.hospitalId + '&serviceType=' + e.serviceType
						})
					} else { //如果优modularId，那么就根据modularId跳转
						switch (this.modularId) {
							case 1: //就医陪诊
								uni.navigateTo({
									url: '/my/customServer/customPz?serviceId=' + e.serviceId + '&serviceType=' + e
										.serviceType
								})
								break;
							case 2: //院内陪护
								uni.setStorageSync('phserviceId', e.serviceId)
								uni.navigateTo({
									url: '/my/customServer/customPeop?name=护理员&item=' + encodeURIComponent(JSON
										.stringify(e))
								})
								break;
							case 3: //优享陪诊
								uni.navigateTo({
									url: '/my/customServer/customPz?serviceId=' + e.serviceId + '&serviceType=' + e
										.serviceType
								})
								break;
							default:
								break;
						}
					}

				} else {
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
				}

			},
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F5F5F5;
	}

	.active {
		color: #4265F5;
		font-size: 38rpx;
	}

	.bgImg {
		width: 100%;
		height: 590rpx;
		position: relative;

		.bgImg-box {
			position: absolute;
			left: 60rpx;
			top: 100rpx;

			.bgImg-box-title {
				font-size: 50rpx;
				color: #ffffff;
				font-weight: bold;
			}

			.bgImg-box-labs {
				width: 635rpx;
				padding: 20rpx 30rpx 20rpx 30rpx;
				// background-color: #4D65FD;
				background-image: linear-gradient(to right, #a5c2fe, #7681ff);
				border-radius: 40rpx;
				color: #ffffff;
				font-size: 24rpx;
				margin-top: 20rpx;
			}
		}
	}

	.info {
		width: 100%;
		height: 212rpx;
		position: absolute;
		bottom: 0;

		.info-box {
			width: 686rpx;
			height: 100%;
			border-radius: 20rpx;
			background-color: #ffffff;

			.info-box-c {
				width: 566rpx;
				height: 100%;
			}

			.info-box-c-t {
				color: #7F8793;
				font-size: 26rpx;

				width: 100%;
				height: 50%;
				overflow: hidden;
				text-overflow: ellipsis;
				display: -webkit-box;
				-webkit-line-clamp: 3;
				-webkit-box-orient: vertical;
				padding-top: 34rpx;
			}

			.info-box-c-b {
				width: 100%;
				height: 50%;
				color: #4265F5;
				font-size: 38rpx;
				font-weight: bold;

				text {
					font-size: 26rpx;
					font-weight: 400;
				}
			}
		}
	}

	.tabs {
		width: 100%;
		margin-top: 40rpx;

		.tabs-box {
			width: 626rpx;
			color: #252525;
			font-size: 26rpx;
			font-weight: 800;

			.tabs-box-l {}

			.tabs-box-r {
				margin-left: 74rpx;
			}
		}
	}

	.cont {
		width: 100%;
		height: auto;
		margin-top: 20rpx;

		.cont-box {
			width: 626rpx;
			height: 100%;
			background-color: #ffffff;
			border-radius: 16rpx;

			.cont-box-c {
				width: 586rpx;
				height: 100%;
				padding-top: 20rpx;
				padding-bottom: 20rpx;
			}
		}
	}

	.btn {
		width: 100%;
		height: 160rpx;
		background-color: #F5F5F5;
		position: fixed;
		bottom: 0;

		.btn-box {
			width: 533rpx;
			height: 76rpx;
			border-radius: 38rpx;
			color: #ffffff;
			background-color: #4D65FD;
		}
	}
</style>