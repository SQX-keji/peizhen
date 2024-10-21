<template>
	<view class="content">
		<!-- 自定义导航栏 -->
		<u-navbar :is-back="false" title="首页" :border-bottom="false" title-color="#ffffff" :background="background">
			<view class="slot-wrap flex align-center" @click="goSelectCity">
				<u-icon name="map-fill" style="margin-left: 20rpx;" color="#ffffff" size="36"></u-icon>
				<text style="margin-left: 10rpx;color: #F7F7F7;font-size: 24rpx;">{{city}}</text>
			</view>
		</u-navbar>
		<!-- 轮播图 -->
		<view class="swiper flex justify-center align-end">
			<view class="swiper-box">
				<swiper :indicator-dots="false" :autoplay="true" style="width: 100%;height: 100%;border-radius: 16rpx;"
					:interval="3000" :duration="1000">
					<swiper-item v-for="(item,index) in swiperList" :key="index"
						@click="goNavSwiper(item.url,item.name,item.describes)">
						<image :src="item.imageUrl" style="width: 100%;height: 100%;border-radius: 16rpx;" mode="">
						</image>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<!-- 金刚区tabs -->
		<!-- <view class="tabs flex justify-center">
			<view class="tabs-box">
				<view class="tabs-box-item flex justify-between align-center">
					<view class="tabs-box-item-c flex justify-center flex-wrap" @click="gotoorder()"
						v-for="(item,index) in tabs" :key="index">
						<image :src="item.img" style="width: 84rpx;height: 84rpx;border-radius: 50%;" mode=""></image>
						<view class="tabs-box-item-c-t">
							{{item.name}}
						</view>
					</view>
				</view>
			</view>
		</view> -->
		<view class="margin" style="border-radius: 24upx;">
			<u-grid :col="5" :border="false">
				<u-grid-item bg-color="#f5f5f5" v-for="(item,index) in gridData" :key='index'
					@click="bindTo(item.url,item.name,item.describes)">
					<image :src="item.imageUrl" style="width: 92rpx;height: 92rpx;border-radius: 92rpx;">
					</image>
					<view style="margin-top: 14rpx;">{{item.name}}</view>
				</u-grid-item>
			</u-grid>
		</view>
		<!-- 分类 -->
		<view class="types flex justify-center">
			<view class="types-box flex flex-wrap justify-between ">
				<view v-for="(item,index) in classType" :key="index" class="types-box-l margin-bottom-sm"
					@click="goNav(item.tags,item.modularId)">
					<image :src="item.image" mode=""></image>
				</view>
			</view>
		</view>

		<!-- 医院列表 -->
		<view class="hospital flex justify-center" v-if="hospitalList.length>0">
			<view class="hospital-box">
				<view class="hospital-box-item flex justify-center"
					@click="goNavs('/my/yxpeizhen/yxpeizhen?hospitalName='+item.hospitalName+'&hospitalId='+item.hospitalId,2)"
					v-for="(item,index) in hospitalList" :key="index">
					<view class="hospital-box-item-c flex align-center justify-between">
						<view class="hospital-box-item-c-l">
							<image :src="item.hospitalImg?item.hospitalImg:'../../static/logo.png'" mode=""></image>
						</view>
						<view class="hospital-box-item-c-r flex flex-wrap align-center">
							<view class="hospital-box-item-c-r-title">{{item.hospitalName}}</view>
							<view class="hospital-box-item-c-r-info flex align-center">
								<text>{{item.hospitalLevel}}</text>|<span>{{item.hospitalType}}</span>
							</view>
							<view class="hospital-box-item-c-r-jj">{{item.hospitalDetails}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="tips">
			没有我要去的医院？<text @click="goNavs('/my/customServer/customServer')">点这儿里</text>
		</view>
		<!-- 新人红包 -->
		<u-popup v-model="popushongbao" mode="center">
			<view>
				<image @tap="GetQuan"
					src="https://songshui.xianmaxiong.com/file/uploadPath/2022/11/28/737f1212e0e0c39fbaccc5b9091a2d73.png"
					style="width: 564upx;height:618upx "></image>
			</view>
		</u-popup>
		<!-- <u-loadmore v-if="hospitalList.length>0" :status="status" :icon-type="iconType" :load-text="loadText" /> -->
		<empty v-if="hospitalList.length<=0" />
		<!-- 悬浮客服按钮 -->
		<image src="../../static/images/kefu.png" @click="selectKeFu()" class="kefu" mode=""></image>
		<uni-popup ref="popusAuthorization" type="center" :maskClick="false">
			<view class="contentview">
				<view class="title">隐私保护指引</view>
				<view class="des" @click="openPrivacyContract">
					在使用当前小程序服务之前，请仔细阅读<text
						style="color: #5074FF;">{{privacyContractName}}</text>。如你同意{{privacyContractName}}，请点击“同意”开始使用。
				</view>
				<view class="btns">
					<button class="item reject" @click="exitMiniProgram">拒绝</button>
					<button id="agree-btn" class="item agree" open-type="agreePrivacyAuthorization"
						@agreeprivacyauthorization="handleAgreePrivacyAuthorization">同意</button>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import empty from '@/components/empty.vue'
	export default {
		components: {
			empty,
		},
		onShareAppMessage(res) {
			return {
				path: '/pages/index/index?invitation=' + this
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
				path: '/pages/index/index?invitation=' + this.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		data() {
			return {
				popushongbao: false,
				status: 'loadmore',
				iconType: 'flower',
				loadText: {
					loadmore: '轻轻上拉',
					loading: '努力加载中',
					nomore: '实在没有了'
				},
				tuiName: '',
				tuiImage: '',
				invitationCode: '',
				background: {
					backgroundColor: '#468EF8',
				},
				swiperList: [],
				gridData: [],
				classType: [],
				myId: '',
				page: 1,
				limit: 10,
				hospitalList: [],
				city: '请选择城市',

				first: true,
				couponNum: '',
				couponId: '',
				newYouhui: 0,
				pages: 1,
				showModal: true,
				arr: [],
				privacyContractName: '',
			}
		},
		onLoad(e) {
			// #ifdef MP-WEIXIN
			try {
				wx.getPrivacySetting({
					success: res => {
						console.log("是否需要授权：", res.needAuthorization, "隐私协议的名称为：", res.privacyContractName)
						if (res.needAuthorization) {
							this.privacyContractName = res.privacyContractName;
							console.log(111111)
							this.$refs.popusAuthorization.open();
						}
					},
					fail: () => {},
					complete: () => {},
				})
			} catch (e) {
				//TODO handle the exception
			}

			// #endif
			let that = this
			that.getClassfly()
			that.getBannerList()
			that.getGrid()
			uni.getLocation({
				type: 'gcj02', //wgs84  gcj02
				success: function(res) {
					console.log(res, '地理位置')
					that.latitude = res.latitude
					that.longitude = res.longitude
					uni.setStorageSync('latitude', res.latitude)
					uni.setStorageSync('longitude', res.longitude)

					// #ifdef MP-WEIXIN
					uni.request({
						url: 'https://apis.map.qq.com/ws/geocoder/v1/?location=' + that.latitude +
							',' + that.longitude + '&key=O2PBZ-6J3CX-GWK44-TXGQL-QKC2T-2UBP6',
						success(re) {
							console.log(re)
							if (re.statusCode === 200) {
								let citydata = re.data.result.address_component.city
								console.log("获取城市名称成功", citydata)
								that.city = citydata ? citydata : '未知'
								uni.setStorageSync('city', citydata)
								that.page = 1
								that.gethospitalList()
							} else {
								console.log("获取信息失败，请重试！")
							}
						}
					});
					// #endif

					// #ifdef APP-PLUS

					if (res.address) {
						that.city = res.address.city
						uni.setStorageSync('city', res.address.city)
					} else {
						that.selectCity(that.longitude, that.latitude);
					}

					that.page = 1
					that.gethospitalList()

					// #endif

					// #ifdef H5
					that.selectCity(that.longitude, that.latitude);
					// #endif


				},
				fail: function() {
					console.log('获取地址失败')
				}
			})
			that.myId = uni.getStorageSync('userId')
			if (that.first && that.myId) {
				that.$Request.getT('/app/common/type/309').then(res => { //新人领取优惠劵Id
					if (res.code == 0) {
						if (res.data && res.data.value) {
							let coupon = res.data.value.split(',')
							that.couponId = coupon[0]
							that.couponNum = coupon[1]
							// console.log(that.couponId,that.couponNum)
						}
					}
				})
				this.$Request.get('/app/common/type/310').then(res => { //是否开启新人优惠券配置  0不开启 1开启
					if (res.code == 0 && res.data.value == 1) {
						that.$Request.get("/app/user/selectUserById").then(res => {
							if (res.code == 0 && res.data) {
								if (res.data.isNewPeople == 1) {
									that.popushongbao = true
									that.first = false
								} else {
									that.first = true
								}
							}
						});
					}
				});

			}

			// 分享
			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}
			// #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
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
			let that = this
			this.city = uni.getStorageSync('city')
			if (this.city) {
				this.page = 1
				this.gethospitalList()
			}
			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				that.$Request.getT('/app/common/type/308').then(res => { //订单变更通知 308
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}
					}
				})
				if (this.showModal) {
					this.openMsg()
				}
			}
			// #endif

		},
		methods: {
			// 打开隐私协议页面
			openPrivacyContract() {
				let that = this;
				wx.openPrivacyContract({
					fail: () => {
						that.$queue.showToast('遇到错误无法打开！');
					}
				})
			},
			// 拒绝隐私协议
			exitMiniProgram() {
				// 直接退出小程序
				wx.exitMiniProgram()
			},
			// 同意隐私协议
			handleAgreePrivacyAuthorization() {
				this.$refs.popusAuthorization.close();
			},
			GetQuan() {
				let data = {
					couponId: this.couponId,
					num: this.couponNum
				}
				this.$Request.get('/app/couponUser/receiveEnvelope', data).then(res => {
					if (res.code == 0) {
						this.first = false
						this.popushongbao = false
						uni.showToast({
							title: '领取成功',
							icon: 'none',
						})
						setTimeout(d => {
							uni.navigateTo({
								url: '/my/youhuijuan/myList'
							});
						}, 1000);
					}
				});
			},
			gotoorder() {
				uni.navigateTo({
					url: '/pages/index/game/orderDet'
				})
			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						this.city = res.data.result ? res.data.result.ad_info.city : '未知'
						uni.setStorageSync('city', this.city)
						this.page = 1
						this.gethospitalList()
					}
				});
			},
			//获取分类
			getClassfly() {
				let data = {
					type: 1
				}
				this.$Request.get('/app/hospitalModular/getModularList', data).then(res => {
					if (res.code == 0) {
						this.classType = res.data.records
					}

				});
			},
			//获取陪诊医院列表
			gethospitalList() {
				let data = {
					page: this.page,
					limit: this.limit,
					city: this.city
				}
				this.$Request.get('/app/hospital/getHospitalList', data).then(res => {
					if (res.code == 0) {
						this.pages = res.data.pages
						if (this.page == 1) {
							this.hospitalList = res.data.records
						} else {
							this.hospitalList = [...this.hospitalList, ...res.data.records]
							// for (let i = 0; i < this.orderList.length; i++) {
							// 	this.orderList[i].gameName = this.orderList[i].gameName.split(",");
							// }
						}
					}
					uni.stopPullDownRefresh();

				});
			},
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 1
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
			},
			// 获取金刚区分类
			getGrid() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 2
				}).then(res => {
					if (res.code == 0) {
						this.gridData = res.data
					}
				});
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
			// 选择城市
			goSelectCity() {
				uni.navigateTo({
					url: '/my/city/city'
				})
			},
			// 跳转
			goNavs(url, type) {
				if (type == 2) {
					uni.navigateTo({
						url: url
					})
					return
				}
				console.log(url, '1111112333')
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (uni.getStorageSync('userId')) {
					uni.navigateTo({
						url: url
					});
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
			//轮播图跳转
			goNavSwiper(url, name, describes) {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (describes && describes === '跳转小程序') {
					let appid = url.substring(0, url.indexOf(','))
					let usls = url.substring(url.indexOf(',') + 1)
					uni.navigateToMiniProgram({
						appId: appid,
						path: usls,
						envVersion: 'release',
						success(res) {
							// 打开成功
							console.log(res);
						}
					})
					return;
				}
				// 判断是否是应用内链接
				if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1 || url.indexOf('/main/') !== -1) {
					uni.navigateTo({
						url
					});
				} else if (url.indexOf('http') !== -1) {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/webView/webView?url=' + url
					})
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				} else if (url.indexOf('客服') !== -1) {
					this.selectKeFu()
				}
			},
			goNav(url, modularId) {
				console.log(url, '1111112333')
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				// 判断是否是应用内链接
				if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1 || url.indexOf('/main/') !== -1) {
					if (uni.getStorageSync('userId')) {
						uni.navigateTo({
							url: url + '?modularId=' + modularId
						});
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
				} else if (url.indexOf('http') !== -1) {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/webView/webView?url=' + url
					})
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				} else if (url.indexOf('客服') !== -1) {
					this.selectKeFu()
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
			bindTo(url, name, describes) {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (describes && describes === '跳转小程序') {
					let appid = url.substring(0, url.indexOf(','))
					let usls = url.substring(url.indexOf(',') + 1)
					uni.navigateToMiniProgram({
						appId: appid,
						path: usls,
						envVersion: 'release',
						success(res) {
							// 打开成功
							console.log(res);
						}
					})
					return;
				}
				let userId = uni.getStorageSync('userId')

				if (url.indexOf('客服') !== -1) {
					this.selectKeFu()

				} else if (url.indexOf('http') !== -1) {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/webView/webView?url=' + url
					})
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				} else {
					if (userId) {
						uni.navigateTo({
							url
						});
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

				}
			}
		},
		onReachBottom: function() {
			if (this.page < this.pages) {
				this.page = this.page + 1;
				this.gethospitalList()
			}

		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.gethospitalList()
		},
	}
</script>

<style lang="scss">
	.u-mode-center-box {
		background-color: '' !important;
	}

	page {
		background-color: #F7F7F7;
	}

	.tips {
		width: 100%;
		text-align: center;

		text {
			color: #5074FF;
		}
	}

	.kefu {
		width: 100rpx;
		height: 100rpx;
		border-radius: 50%;
		position: fixed;
		bottom: 200rpx;
		right: 40rpx;
	}

	.content {
		padding-bottom: 50rpx;
	}

	.swiper {
		width: 100%;
		height: 320rpx;
		background-image: linear-gradient(to bottom, #468EF8, #ffffff);

		.swiper-box {
			width: 686rpx;
			height: 300rpx;
			border-radius: 16rpx;
		}
	}

	.tabs {
		width: 100%;
		height: 130rpx;
		margin-top: 20rpx;

		.tabs-box {
			width: 686rpx;
			height: 100%;

			.tabs-box-item-c-t {
				font-size: 24rpx;
				color: #000000;
				margin-top: 14rpx;
			}
		}
	}

	.types {
		width: 100%;
		// height: 165rpx;
		margin-top: 30rpx;

		.types-box {
			width: 686rpx;
			height: 100%;

			.types-box-l {
				width: 333rpx;
				height: 100%;
				border-radius: 16rpx;

				image {
					width: 100%;
					height: 165rpx;
					border-radius: 16rpx;
				}
			}

			.types-box-r {
				width: 333rpx;
				height: 100%;
				border-radius: 16rpx;

				image {
					width: 100%;
					height: 100%;
					border-radius: 16rpx;
				}
			}
		}
	}

	.hospital {
		width: 100%;
		height: auto;
		// margin-top: 20rpx;

		.hospital-box {
			width: 686rpx;
			height: auto;

			.hospital-box-item {
				width: 100%;
				height: 220rpx;
				border-radius: 16rpx;
				background-color: #ffffff;
				margin-bottom: 20rpx;
			}

			.hospital-box-item-c {
				width: calc(686rpx - 60rpx);
				height: 100%;
			}

			.hospital-box-item-c-l {
				width: 200rpx;
				height: 160rpx;

				image {
					width: 100%;
					height: 100%;
					border-radius: 10rpx;
				}
			}

			.hospital-box-item-c-r {
				width: 406rpx;
				height: 160rpx;
			}

			.hospital-box-item-c-r-title {
				width: 100%;
				color: #1E1F31;
				font-size: 32rpx;
				font-weight: bold;
			}

			.hospital-box-item-c-r-info {
				width: 100%;
				color: #666666;
				font-size: 26rpx;
				font-weight: 500;

				text {
					color: #FF574E;
					margin-right: 10rpx;
				}

				span {
					margin-left: 10rpx;
				}
			}

			.hospital-box-item-c-r-jj {
				width: 100%;
				color: #666666;
				font-size: 26rpx;
				font-weight: 500;
				overflow: hidden;
				white-space: nowrap;
				text-overflow: ellipsis;
			}
		}
	}

	.contentview {
		width: 632rpx;
		padding: 48rpx;
		box-sizing: border-box;
		background: #fff;
		border-radius: 16rpx;
	}

	.contentview .title {
		text-align: center;
		color: #333;
		font-weight: bold;
		font-size: 32rpx;
	}

	.contentview .des {
		font-size: 26rpx;
		color: #666;
		margin-top: 40rpx;
		text-align: justify;
		line-height: 1.6;
	}

	.contentview .des .link {
		color: #5074FF;
		text-decoration: underline;
	}

	button::after {
		border: none;
	}

	.btns {
		margin-top: 48rpx;
		display: flex;
	}

	.btns .item {
		justify-content: space-between;
		width: 244rpx;
		height: 80rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 16rpx;
		box-sizing: border-box;
		border: none;
	}

	.btns .reject {
		background: #f4f4f5;
		color: #909399;
	}

	.btns .agree {
		background: #5074FF;
		color: #fff;
	}
</style>