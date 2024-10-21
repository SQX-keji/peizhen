<template>
	<view style="padding-bottom: 50rpx;">
		<!-- 背景图 -->
		<view class="bg">
			<image :src="swiperList[0]?swiperList[0].imageUrl:''" mode=""></image>
		</view>
		<!-- 服务列表 -->

		<view style="margin-top: -49rpx;">
			<view class="list flex justify-center" v-for="(item,index) in classType" :key="index" @click="goNav(item)">
				<view class="list-box flex justify-center align-center">
					<view class="list-box-c flex justify-between align-center">
						<view class="list-box-c-l flex align-center">
							<view class="list-box-c-l-c">
								<view class="list-box-c-l-c-t">{{item.serviceName}}</view>
								<view class="list-box-c-l-c-t-b">
									{{item.title}}
								</view>
							</view>
						</view>
						<view class="list-box-c-r">
							<span>¥</span><text>{{item.money}}</text>元/{{item.company==1?'天':'次'}}
						</view>
					</view>
					<view class="list-box-tips">
						{{item.modularId==1?'就医陪诊':(item.modularId==2?'院内陪护':'优享陪诊')}}
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				classType: [],
				hospitalName: '',
				hospitalId: '',
				swiperList: [],
				tuiName: '',
				tuiImage: '',
				invitationCode: '',
			};
		},
		onShareAppMessage(res) {
			return {
				path: '/my/customServer/customServer?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		/*
		 * uniapp微信小程序分享页面到微信朋友圈
		 */
		onShareTimeline(res) {
			return {
				path: '/my/customServer/customServer?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		onLoad(option) {
			let that = this
			this.getBannerList()
			//就医陪诊
			this.getlist(1)
			//院内陪护
			this.getlist(2)
			//优享陪诊
			this.getlist(3)
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
		},
		onShow() {

		},
		methods: {
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 9
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
			},
			goNav(e) {
				if (uni.getStorageSync('token')) {
					uni.navigateTo({
						url: '/my/yxpeizhen/info?data=' + encodeURIComponent(JSON.stringify(e)) + '&modularId=' + e
							.modularId,

					})
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
			//获取服务列表 modularId: 1、就医陪诊 2、院内陪护 3、优享陪诊
			getlist(modularId) {
				let data = {
					modularId: modularId
				}
				this.$Request.get('/app/hospitalEmploy/getHospitalEmployList', data).then(res => {
					if (res.code == 0) {

						res.data.records.map(item => {
							if (item.tags) {
								item.tags = item.tags.split(',')
							} else {
								item.tag = []
							}
						})
						this.classType = [...this.classType, ...res.data.records]
					}

				});
			}
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #F5F5F5;
	}

	.bg {
		width: 100%;
		height: 280rpx;
		z-index: 1;

		image {
			width: 100%;
			height: 100%;
			z-index: 1;
		}
	}

	.list {
		width: 100%;
		z-index: 2;
		margin-bottom: 20rpx;


		.list-box {
			width: 686rpx;
			padding: 40rpx 0;
			background-color: #ffffff;
			border-radius: 16rpx;
			z-index: 2;
			position: relative;

			.list-box-tips {
				position: absolute;
				top: 0;
				right: 0;
				padding: 4rpx 30rpx;
				background-color: #5074FF;
				color: #ffffff;
				border-radius: 0 16rpx 0 16rpx;
			}

			.list-box-c {
				width: 606rpx;
				// height: 85rpx;
			}

			.list-box-c-l {
				width: 60%;

				image {
					width: 85rpx;
					height: 85rpx;
				}
			}

			.list-box-c-l-c-t {
				color: #333333;
				font-size: 32rpx;
				font-weight: bold;
			}

			.list-box-c-l-c-t-b {
				color: #999999;
				font-size: 26rpx;
				margin-top: 8rpx;
				display: flex;
				align-items: center;
			}

			.list-box-c-r {
				color: #FF2D01;
				font-size: 32rpx;
				letter-spacing: 5rpx;

				span {
					font-size: 38rpx;
					font-weight: bold;
				}

				text {
					font-size: 52rpx;
					font-weight: bold;
					letter-spacing: 0rpx;
				}
			}
		}
	}
</style>