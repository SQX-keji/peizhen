<template>
	<view>
		<view class="header flex justify-center">
			<view class="header-box">
				<view class="header-box-info flex align-center justify-between">
					<view class="flex align-center">
						<image :src="avatar" mode=""></image>
						<view class="header-box-info-b" v-if="!isLogin">
							<view class="header-box-info-name">
								{{userName}}
							</view>
							<view class="header-box-info-id">
								ID：{{invitationCode}}
							</view>
						</view>
						<view class="header-box-info-b" v-else @click="goLogin('/pages/public/login')">
							<view class="header-box-info-name">
								登录
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 订单 -->
		<view class="order flex justify-center" v-if="XCXIsSelect != '否'" @click="goNavs('/pages/order/index')">
			<view class="order-box">
				<view class="order-box-title flex justify-between align-center">
					<text style="margin-left: 30rpx;color: #1A1A1A;font-size: 32rpx;font-weight: 800;">我的订单</text>
					<u-icon name="arrow-right" color="#1A1A1A" size="32" style="margin-right: 30rpx;"></u-icon>
				</view>
				<view class="order-box-box flex justify-around align-center">
					<view class="order-box-box-item flex justify-center flex-wrap"
						@click.stop="goNavsTab('/pages/order/index',1)">
						<image src="../../static/images/dfk.png" style="width: 50rpx;height: 50rpx;" mode=""></image>
						<view class="" style="margin-top: 10rpx;color: #1A1A1A;font-size: 24rpx;">
							待付款
						</view>
					</view>
					<view class="order-box-box-item flex justify-center flex-wrap"
						@click.stop="goNavsTab('/pages/order/index',4)">
						<image src="../../static/images/jxz.png" style="width: 50rpx;height: 50rpx;" mode=""></image>
						<view class="" style="margin-top: 10rpx;color: #1A1A1A;font-size: 24rpx;">
							进行中
						</view>
					</view>
					<view class="order-box-box-item flex justify-center flex-wrap"
						@click.stop="goNavsTab('/pages/order/index',5)">
						<image src="../../static/images/ywc.png" style="width: 50rpx;height: 50rpx;" mode=""></image>
						<view class="" style="margin-top: 10rpx;color: #1A1A1A;font-size: 24rpx;">
							已完成
						</view>
					</view>
					<view class="order-box-box-item flex justify-center flex-wrap"
						@click.stop="goNavsTab('/pages/order/index',6)">
						<image src="../../static/images/yqx.png" style="width: 50rpx;height: 50rpx;" mode=""></image>
						<view class="" style="margin-top: 10rpx;color: #1A1A1A;font-size: 24rpx;">
							已取消
						</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 推荐工具 -->
		<view class="utils flex justify-center">
			<view class="utils-box">
				<view class="flex align-center flex-wrap"
					style="margin-left: 25rpx;margin-top: 20rpx;margin-bottom: 20rpx;">
					<view class="utils-box-item flex justify-center align-center flex-wrap"
						@click="goNav('/my/other/car?type=4')">
						<image src="../../static/images/my/che.png" style="width: 56rpx;height: 56rpx;" mode=""></image>
						<view class="" style="color: #333333;font-size: 26rpx;margin-top: 15rpx;">
							就诊人管理
						</view>
					</view>
					<view class="utils-box-item flex justify-center align-center flex-wrap"
						@click="goNav('/my/address/address')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/address.png" style="width: 56rpx;height: 56rpx;" mode="">
						</image>
						<view class="" style="color: #333333;font-size: 26rpx;margin-top: 15rpx;">
							地址管理
						</view>
					</view>
					<view class="utils-box-item flex justify-center align-center flex-wrap" @click="goOut">
						<image src="../../static/images/my/set.png" style="width: 56rpx;height: 56rpx;" mode=""></image>
						<view class="" style="color: #333333;font-size: 26rpx;margin-top: 15rpx;">
							退出登录
						</view>
					</view>
				</view>
			</view>
		</view>

		<view class="margin" @click="goYouhuiImage(youhuiImage.url)" v-if="youhuiImage">
			<image :src="youhuiImage.imageUrl" style="width: 100%;height: 220rpx;"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				avatar: '../../static/logo.png',
				isLogin: true,
				userName: '匿名',
				browse: 0, //浏览数
				fans: 0, //粉丝数
				follow: 0, //关注数
				visitor: 0, //访客数
				userId: '',
				isVip: false,
				invitationCode: '', //邀请码
				czSel: '否',
				geRen: 0,
				Qe: 0,
				XCXIsSelect: '否',
				renzheng: false,
				isPromotion: '', //推广员  1是  0或者null 不是
				isAgent: '', //代理商  1是  0或者null 不是
				yhq: 0, //优惠券数量
				yue: 0, //余额
				ts: 0, //投诉数量
				showModal: true,
				arr: [],
				appID: '',
				dailishang: '否',
				youhuiImage: ''
			}
		},
		onLoad() {
			this.invitationCode = this.$queue.getData('invitationCode');
			this.dailishang = this.$queue.getData('dailishang')
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			this.$Request.getT('/app/common/type/239').then(res => { //陪诊师傅端微信小程序APPID 248
				if (res.code === 0) {
					this.appID = res.data.value;
				}
			});
		},
		onShow() {
			this.$Request.get("/app/banner/selectBannerList", {
				classify: 10
			}).then(res => {
				if (res.code == 0) {
					this.youhuiImage = res.data[0]
				}
			});
			this.czSel = this.$queue.getData('czSel');
			this.Qe = uni.getStorageSync("Qe")
			this.geRen = uni.getStorageSync("geRen")
			if (this.Qe == 2 || this.geRen == 2) {
				this.renzheng = false
			} else {
				this.renzheng = true
			}
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.isLogin = false
				this.getUserInfo()
				this.getRenZheng()
				this.getRenZhengs()
				this.getAmount()
				this.getIsVip()
				this.getuhNum()
				this.$Request.getT('/app/common/type/308').then(res => { //下单成功
					if (res.code == 0) {
						if (res.data && res.data.value) {
							this.arr.push(res.data.value)
						}
					}
				})
				// #ifdef MP-WEIXIN
				//订阅
				if (this.showModal) {
					this.openMsg()
				}
				// #endif
			} else {
				this.isLogin = true
				this.userName = '匿名'
				this.browse = 0
				this.fans = 0
				this.follow = 0
				this.visitor = 0
				this.yhq = 0
				this.yue = 0
				this.ts = 0
				this.avatar = '../../static/logo.png'
			}

		},
		methods: {
			goOut() {
				uni.showModal({
					title: '提示',
					content: '确定退出登录吗？',
					success: function(res) {
						if (res.confirm) {
							console.log('用户点击确定');
							uni.removeStorageSync('userName')
							uni.removeStorageSync('avatar')
							uni.removeStorageSync('userId')
							uni.removeStorageSync('token')
							uni.removeStorageSync('phone')
							uni.removeStorageSync('zhiFuBaoName')
							uni.removeStorageSync('zhiFuBao')
							uni.removeStorageSync('invitationCode')
							uni.removeStorageSync('unionId')
							uni.removeStorageSync('openId')
							uni.removeStorageSync('isVIP')
							uni.showToast({
								title: '退出成功！',
								icon: 'none'
							})
							setTimeout(function() {
								uni.navigateBack()
							}, 1000)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				})
			},
			goYouhuiImage(url) {
				uni.navigateTo({
					url: url
				})
			},
			//跳转小程序
			goRider() {
				let token = this.$queue.getData("token");
				if (token) {
					// #ifdef MP-WEIXIN
					let that = this
					uni.navigateToMiniProgram({
						appId: that.appID,
						path: '/pages/index/index',
						extraData: {
							'data1': 'test'
						},
						success(res) {
							// 打开成功
							console.log("打开成功")
						}
					})
					// #endif
					// #ifdef H5
					this.$Request.get('/app/common/type/321').then(res => {
						if (res.code == 0 && res.data.value) {
							uni.showModal({
								title: '提示',
								content: '请跳转至' + res.data.value + '师傅端进行入驻',
								confirmText: '一键复制',
								cancelText: '取消',
								complete(ret) {
									if (ret.confirm) {
										uni.setClipboardData({
											data: res.data.value,
											success(re) {
												uni.showToast({
													title: '复制成功',
													icon: 'none'
												})
											}
										})
									}
								}
							})
						} else {
							uni.showToast({
								title: '请联系客服',
								icon: 'none'
							})
						}
					});
					// #endif
					// #ifdef APP
					this.$Request.get('/app/common/type/320').then(res => {
						if (res.code == 0 && res.data.value) {
							uni.showModal({
								title: '提示',
								content: '请跳转至' + res.data.value + '下载师傅端进行入驻',
								confirmText: '一键复制',
								cancelText: '取消',
								complete(ret) {
									if (ret.confirm) {
										uni.setClipboardData({
											data: res.data.value,
											success(re) {
												uni.showToast({
													title: '复制成功',
													icon: 'none'
												})
											}
										})
									}
								}
							})
						} else {
							uni.showToast({
								title: '请联系客服',
								icon: 'none'
							})
						}
					});
					// #endif

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
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						console.log(ret.subscriptionsSetting, '------------------')
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							console.log(99999)
							uni.setStorageSync('sendMsg', false)
							uni.showModal({
								title: '提示',
								content: '为了更好的体验,请绑定消息推送',
								confirmText: '确定',
								cancelText: '取消',
								success: function(res) {
									if (res.confirm) {
										wx.requestSubscribeMessage({
											tmplIds: that.arr,
											success(re) {
												console.log(JSON.stringify(re),
													'++++++++++++++')
												var datas = JSON.stringify(re);
												if (datas.indexOf("accept") != -1) {
													console.log(re)
													// uni.setStorageSync('sendMsg', true)
												}
											},
											fail: (res) => {
												console.log(res)
											}
										})
										// uni.setStorageSync('sendMsg', true)
										console.log('确认')
										that.showModal = false
									} else if (res.cancel) {
										console.log('取消')
										// uni.setStorageSync('sendMsg', false)
										that.showModal = true
									}
								}
							})
						}
					}
				})
			},
			// 优惠券
			getuhNum() {
				this.$Request.getT("/app/user/getUserData").then(res => {
					if (res.code == 0) {
						this.yhq = res.data.couponCount
						this.yue = res.data.userMoney
						this.ts = res.data.messageCount
					}
				});
			},
			//订单跳转
			goNavsTab(e, tabIndex) {
				console.log(e)
				if (this.userId) {
					getApp().globalData.tabIndex = tabIndex
					uni.switchTab({
						url: e
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
			//跳转客服
			selectKeFu() {
				let SelKeFu = this.$queue.getData('SelKeFu');
				if (SelKeFu + '' == '1') { //手机号
					uni.makePhoneCall({
						phoneNumber: uni.getStorageSync('kefuPhone')
					});
				} else if (SelKeFu + '' == '2') { //企业微信
					let that = this
					// #ifdef MP-WEIXIN
					wx.openCustomerServiceChat({
						extInfo: {
							url: that.$queue.getData('kefu')
						},
						corpId: that.$queue.getData('kefuAppId'),
						success(res) {}
					})
					// #endif
					// #ifdef H5
					window.location.href = that.$queue.getData('kefu');
					// #endif
					// #ifdef APP
					let kefu = that.$queue.getData('kefu')
					console.log(kefu)
					plus.runtime.openURL(kefu, function(res) {});
					// #endif
				} else { //客服二维码页面
					uni.navigateTo({
						url: '/pages/kefu/kefu'
					})
				}
			},
			goNav(e, type) {
				console.log(e)
				if (type == 'noLogin') {
					uni.navigateTo({
						url: e
					})
					return
				}

				if (this.userId) {
					uni.navigateTo({
						url: e
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
			goNavs(e, name) {
				console.log(e)
				if (this.userId) {
					uni.switchTab({
						url: e
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
			goLogin(e) {
				uni.navigateTo({
					url: e
				})
			},
			getAmount() {
				this.$Request.get("/app/userBrowse/selectAmount").then(res => {
					if (res.code == 0) {
						this.browse = res.data.browse
						this.fans = res.data.fans
						this.follow = res.data.follow
						this.visitor = res.data.visitor
					}
				});
			},
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.userName = res.data.userName
						this.invitationCode = res.data.invitationCode
						this.avatar = res.data.avatar ? res.data.avatar : '../../static/logo.png'
						this.isAuthentication = res.data.isAuthentication

						if (res.data.isPromotion == null || res.data.isPromotion == 0) { //推广员
							this.isPromotion = '0'
						} else {
							this.isPromotion = res.data.isPromotion
						}
						if (res.data.isAgent == null || res.data.isAgent == 0) { //代理商
							this.isAgent = '0'
						} else {
							this.isAgent = res.data.isAgent
						}

						uni.setStorageSync('feiRate', res.data.feiRate) //代理商佣金比例
						uni.setStorageSync('isAuthentication', res.data.isAuthentication)
						uni.setStorageSync('avatar', res.data.avatar)
						uni.setStorageSync('invitationCode', res.data.invitationCode)
						uni.setStorageSync('zhiFuBao', res.data.zhiFuBao)
						uni.setStorageSync('zhiFuBaoName', res.data.zhiFuBaoName)

					}
				});
			},
			// 个人认证数据
			getRenZheng() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 1).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) {
							this.geRen = 0
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 0) {
							this.geRen = 1
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 1) {
							this.geRen = 2
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 2) {
							this.geRen = 3
							uni.setStorageSync("geRen", this.geRen)
						}
					}
				});
			},
			// 企业认证数据
			getRenZhengs() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 2).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) { //未实名
							this.Qe = 0
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 0) { //审核中
							this.Qe = 1
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 1) { //已实名
							this.Qe = 2
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 2) { //已拒绝
							this.Qe = 3
							uni.setStorageSync("Qe", this.Qe)
						}
					}
				});
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			}

		}
	}
</script>

<style lang="scss">
	page {
		background: #F7F6F5;
	}

	.header {
		width: 100%;
		// height: 350rpx;
		padding-bottom: 20rpx;
		background-image: linear-gradient(#e1edff, #F7F6F5);

		// background-color: #e1edff;
		.header-box {
			width: 686rpx;
			height: 100%;

			.money {
				width: 686rpx;
				height: 170rpx;
				background-color: #ffffff;
				border-radius: 24rpx;
				margin-top: 40rpx;

				.money-item {
					width: calc(100% / 3);
					text-align: center;

					.money-item-price {
						width: 100%;
						color: #333333;
						font-size: 44rpx;
						font-weight: bold;
					}

					.money-item-title {
						width: 100%;
						color: #333333;
						font-size: 24rpx;
					}
				}
			}

			.header-box-info {
				width: 100%rpx;
				height: 100rpx;
				margin-top: 80rpx;

				image {
					width: 100rpx;
					height: 100rpx;
					border-radius: 50%;
				}

				.header-box-info-b {
					margin-left: 16rpx;
				}

				.header-box-info-name {
					color: #0C0C0C;
					font-size: 38rpx;
					font-weight: bold;
				}

				.header-box-info-id {
					color: #0C0C0C;
					font-size: 28rpx;
				}
			}

		}
	}

	.header-box-tg {
		margin-top: 20rpx;
		width: 100%;
		height: 150rpx;

		.header-box-tg1 {
			width: 686rpx;
			height: 100%;
			background: #ffffff;
			border-radius: 24rpx;
		}

		.header-box-tg-c {
			width: 626rpx;
			height: 90rpx;
		}

		.header-box-tg-c-l {
			width: 50%;
			height: 100%;
			border-right: 1px solid #E6E6E6;
			box-sizing: border-box;
		}

		.header-box-tg-c-r {
			width: 50%;
			height: 100%;
		}
	}

	.order {
		width: 100%;
		height: 236rpx;

		// margin-top: 20rpx;
		.order-box {
			width: 686rpx;
			height: 100%;
			border-radius: 24rpx;
			background-color: #ffffff;

			.order-box-title {
				width: 100%;
				height: 88rpx;
			}

			.order-box-box {
				width: 100%;
				height: 146rpx;

				.order-box-box-item {
					width: 80rpx;
					height: 100rpx;
				}
			}
		}
	}

	.utils {
		width: 100%;
		margin-top: 20rpx;

		.utils-box {
			width: 686rpx;
			background-color: #ffffff;
			border-radius: 24rpx;

			.utils-box-item {
				width: 140rpx;
				margin-bottom: 20rpx;
				margin-right: 20rpx;
			}
		}
	}
</style>