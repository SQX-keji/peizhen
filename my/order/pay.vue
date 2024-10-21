<template>
	<view>
		<view class="padding-lr padding-bottom">
			<view class="status" v-if="order.state==0">订单待付款...</view>
			<view class="status" v-if="order.state==1">订单进行中...</view>
			<view class="status" v-if="order.state==2">订单已完成...</view>
			<view class="status" v-if="order.state==3">订单已取消...</view>
			<view class="status" v-if="order.state==4">订单待接单...</view>
			<view class="status" v-if="order.state==5">订单待服务...</view>

			<view class="types flex align-center justify-between">
				<view class="types-box-l flex align-center" style="width: 60%;" v-if="order.appointInformation">
					<image :src="order.appointInformation.img?order.appointInformation.img: '../../static/logo.png'"
						style="width: 74rpx;height: 66rpx;margin-left: 44rpx;" mode="">
					</image>
					<view class="types-r">
						<view class="types-r-t">{{order.appointInformation.serviceName}}</view>
						<view class="types-r-b flex flex-wrap align-center">
							<view v-for="(ite,ind) in order.appointInformation.tags" :key="ind">
								{{ite}} <text v-if="ind!=order.appointInformation.tags.length-1"
									class="padding-lr-xs">|</text>
							</view>
						</view>
					</view>
				</view>
				<view class="types-box-r flex align-center">
					{{order.state!=0 && order.state!=3?'实付：':''}}  
					<text>¥</text><span>{{order.payMoney}}</span>
				</view>
			</view>
			<view @click="goInfo(order.orderTakingUserId)" class="bg padding radius margin-top" v-if="order.state == 1 || order.state == 2 || order.state == 5">
				<view>联系接单人</view>
				<view class="flex align-center margin-top">
					<view>
						<image :src="order.rideAvatar?order.rideAvatar:'../../static/logo.png'"
							style="width: 80rpx;height: 80rpx;border-radius: 60upx;"></image>
					</view>
					<view class="text-white margin-left-sm" @click.stop="bindphone(order.ridePhone)">
						<view>{{order.rideUserName}}</view>
						<view class="margin-top-xs">{{order.ridePhone.replace(/^(\d{3})\d{4}(\d{4})$/, "$1****$2")}}	<u-icon name="phone"></u-icon></view>
					</view>
				</view>
			</view>

			<view class="bg padding radius margin-top" v-if="order.appointInformation">
				<view class="text-lg text-bold">
					预约信息
				</view>
				<view class=" margin-right-xs">
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 180upx;color: #999999;">期望就诊时间</view>
						<view>{{order.appointInformation.hopeTime}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">就诊医院</view>
						<view>{{order.appointInformation.hospitalName}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.departmentName">
						<view class="" style="width: 150upx;color: #999999;">就诊科室</view>
						<view>{{order.appointInformation.departmentName}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.badNo">
						<view class="" style="width: 150upx;color: #999999;">就诊床号</view>
						<view>{{order.appointInformation.badNo}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.nursingNeeds">
						<view class="" style="width: 150upx;color: #999999;">护理需求</view>
						<view>{{order.appointInformation.nursingNeeds}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.symptom">
						<view class="" style="width: 150upx;color: #999999;">病症</view>
						<view>{{order.appointInformation.symptom}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.selfAbility">
						<view class="" style="width: 150upx;color: #999999;">自理能力</view>
						<view>{{order.appointInformation.selfAbility}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.serviceNum">
						<view class="" style="width: 150upx;color: #999999;">服务天数</view>
						<view>{{order.appointInformation.serviceNum}}天</view>
					</view>
					
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">就诊人</view>
						<view>{{order.appointInformation.patientInfo?order.appointInformation.patientInfo.realName:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">就诊关系</view>
						<view>{{order.appointInformation.patientInfo?order.appointInformation.patientInfo.relationship:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.phone">
						<view class="" style="width: 150upx;color: #999999;">联系电话</view>
						<view>{{order.appointInformation.phone}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">就诊人电话</view>
						<view>{{order.appointInformation.patientInfo?order.appointInformation.patientInfo.phone:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.patientInfo && order.appointInformation.patientInfo.emergencyPhone">
						<view class="" style="width: 150upx;color: #999999;">紧急联系人</view>
						<view>{{order.appointInformation.patientInfo?order.appointInformation.patientInfo.emergencyPhone:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.drugsType">
						<view class="" style="width: 150upx;color: #999999;">药物类型</view>
						<view>{{order.appointInformation?order.appointInformation.drugsType:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.drugsName">
						<view class="" style="width: 150upx;color: #999999;">药物名称</view>
						<view>{{order.appointInformation?order.appointInformation.drugsName:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.reportType">
						<view class="" style="width: 150upx;color: #999999;">报告信息</view>
						<view>{{order.appointInformation?order.appointInformation.reportType:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.exclusiveType">
						<view class="" style="width: 150upx;color: #999999;">专享类型</view>
						<view>{{order.appointInformation?order.appointInformation.exclusiveType:'暂无'}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation.userDetailsAddress">
						<view class="" style="width: 150upx;color: #999999;">{{order.appointInformation.serviceType==1?'接送地址':'收件地址'}}</view>
						<view>{{order.appointInformation.userProvince}}{{order.appointInformation.userCity}}{{order.appointInformation.userDistrict}}{{order.appointInformation.userDetailsAddress}}</view>
					</view>
					<!-- <view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">服务天数</view>
						<view>{{order.appointInformation.serviceNum?order.appointInformation.serviceNum+'天':'暂无'}}</view>
					</view> -->
					<view class="margin-top-lg" v-if="order.appointInformation && order.appointInformation.imgRemarks" style="width: 165rpx;color: #999999;">图片资料</view>
					<view class="margin-top-lg flex flex-wrap" v-if="order.appointInformation && order.appointInformation.imgRemarks">
						<image @click="peiveImgs(index,order.appointInformation.imgRemarks.split(','))" :src="item" style="width: 195rpx;height: 195rpx;border-radius: 10rpx;margin-right: 10rpx;margin-bottom: 10rpx;" v-for="(item,index) in order.appointInformation.imgRemarks?order.appointInformation.imgRemarks.split(','):[]" mode=""></image>
					</view>
					<view class=" margin-top-lg">
						<view class="" style="width: 165rpx;color: #999999;">特殊需求</view>
						<view class="text-white margin-top">{{order.appointInformation.remarks?order.appointInformation.remarks:'暂无'}}</view>
					</view>
				</view>
			</view>
			<view class="bg padding radius margin-top" style="margin-bottom: 140rpx;">
				<view class="text-lg text-bold">
					订单信息
				</view>
				<view class="margin-right-xs">
					<view class="flex justify-between margin-top-lg" v-if="order.appointInformation">
						<view class="" style="width: 150upx;color: #999999;">服务类型</view>
						<view class="text-white flex align-center">{{order.appointInformation.serviceName}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">订单编号</view>
						<view class="text-white flex align-center">{{order.ordersNo}}
							<image src="../static/copy.png" style="width: 45rpx;height: 45rpx;margin-left: 5upx;"
								@click.stop="copyClick(order.ordersNo)"></image>
						</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">下单时间</view>
						<view class="text-white">{{order.createTime}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.orderMoney>order.payMoney">
						<view class="" style="width: 150upx;color: #999999;">优惠金额</view>
						<view class="text-white">￥{{order.orderMoney - order.payMoney}}</view>
					</view>
				</view>
			</view>

			<view class="box" v-if="showPay">
				<view class="bottbox">
					<view class="flex align-start justify-between">
						<view
							style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
							选择支付方式
						</view>
						<view class="margin-right margin-top-sm" @click="bindclost">
							<image src="../static/colse.png" style="width: 60upx;height: 60upx;"></image>
						</view>
					</view>
					<view style="display: flex;" v-for="(item,index) in openLists" :key='index'>
						<view style="width: 100%;display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
							v-if="item.text === '零钱' && czSel != '否'">
							<view style="display: flex;width:80%;align-items: center;">
								<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
								<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
								</view>
							</view>
							<view style="width: 20%;display: flex;justify-content: flex-end;">
								<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
									<label class="tui-radio">
										<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
									</label>
								</radio-group>
							</view>
						</view>
						<view style="width: 100%;display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
							v-if="item.text != '零钱'">
							<view style="display: flex;width:80%;align-items: center;">
								<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
								<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
								</view>
							</view>
							<view style="width: 20%;display: flex;justify-content: flex-end;">
								<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
									<label class="tui-radio">
										<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
									</label>
								</radio-group>
							</view>
						</view>
					</view>
					<view
						style="width: 690rpx;height: 80rpx;background:#1789FD;color:#FFFFFF;text-align: center;line-height: 80rpx;border-radius: 50rpx;margin: 30rpx;"
						@click="pay()">确认支付</view>
				</view>

			</view>
			<!-- <u-popup v-model="showPay" mode="bottom" border-radius="14" :closeable="true">
				<view>
					<view
						style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
						选择支付方式
					</view>
					<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
						v-for="(item,index) in openLists" :key='index'>
						<view style="display: flex;width:80%;align-items: center;">
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
							</view>
						</view>
						<view style="width: 20%;display: flex;justify-content: flex-end;">
							<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
								<label class="tui-radio">
									<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
					<view
						style="width: 690rpx;height: 80rpx;background:#1789FD;color:#FFFFFF;text-align: center;line-height: 80rpx;border-radius: 50rpx;margin: 30rpx;"
						@click="pay()">确认支付</view>
				</view>
			</u-popup>
 -->
			<view class="flex tabber padding-top-sm padding-bottom-sm align-center"
				v-if="order.state == 0||order.state == 3">
				<u-button @click="cancelOrder(order)" shape="circle" class="margin-right" :custom-style="customStyle"
					:hair-line="false" v-if="order.state == 0">取消订单
				</u-button>
				<!-- <view class="btn" @click="openBox()" v-if="order.state == 0">立即支付</view> -->
				<u-button class="margin-right" shape="circle" :custom-style="customStyle2" :hair-line="false"
					@click="showPay=true" v-if="order.state == 0">立即支付
				</u-button>
				<u-button v-if="order.state == 3" class="margin-right" :custom-style="customStyle" shape="circle"
					:plain="true" @click="delOrder(order)">删除订单</u-button>
				<u-button v-if="order.state == 1" class="margin-right" :custom-style="customStyle" shape="circle"
					:plain="true" @click="cancel(item)" 订单完成</u-button>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				isPay:true,
				customStyle: {
					backgroundColor: '#557EFD',
					color: '#FFFFFF',
					width: "200upx",
					margin: "0 30upx 0upx 0"
				},
				customStyle2: {
					backgroundColor: '#557EFD',
					color: '#FFFFFF',

					width: "200upx",
					margin: "0 30upx 0upx 0"
				},
				id: '',
				order: {
					user: {},
					game: {}
				},
				isTrue: 0,
				czSel: '否',
				phone: '',
				isVip: false,
				showPay: false,
				openWay: 0,
				openLists: [],

			}
		},
		onLoad(e) {
			this.czSel = this.$queue.getData('czSel');
			this.isTrue = e.isTrue
			if (this.isTrue) {
				uni.setNavigationBarTitle({
					title: '订单详情'
				})
			}
			this.id = e.id
			// #ifdef APP-PLUS
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 2;
			// #endif

			// #ifdef H5
			let ua = navigator.userAgent.toLowerCase();
			if (ua.indexOf('micromessenger') !== -1) {
				this.openLists = [{
					image: '/static/images/zhifubao.png',
					text: '支付宝',
					id: 1
				}, {
					image: '/static/images/lingqian.png',
					text: '零钱',
					id: 3
				}, {
					image: '/static/images/icon_weixin.png',
					text: '微信',
					id: 2
				}];
				this.openWay = 1;
			} else {
				this.openLists = [{
					image: '/static/images/zhifubao.png',
					text: '支付宝',
					id: 1
				}, {
					image: '/static/images/lingqian.png',
					text: '零钱',
					id: 3
				}];
				this.openWay = 1;
			}
			// #endif
			// this.getOrder()
		},
		onShow() {
			this.showPay = false
			this.getOrder()
			this.token = uni.getStorageSync('token')
			if (uni.getStorageSync('token')) {
				this.getIsVip()
			}
		},
		methods: {
			//预览图片
			peiveImgs(index,imgs){
				uni.previewImage({
					current:index,
					urls:imgs
				})
			},
			goInfo(userId){
				uni.navigateTo({
					url:'/my/introduction/introduction?userId='+userId
				})
			},
			bindclost() {
				this.showPay = false
			},
			// 查看图片
			saveImg(imgs, index) {
				if (this.LBSelect) {
					if (index == this.LBIndex - 1) {
						return;
					}
				}
				// console.log(imgs)
				let that = this;
				let imgArr = imgs
				// imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
			copyClick(copy) {
				uni.setClipboardData({
					data: copy,
					success: function(res) {
						uni.getClipboardData({
							success: function(res) {
								uni.showToast({
									title: "复制成功",
									icon: 'none',
								});
							},
						});
					},
				});
			},
			// 一键导航
			bindGps(latitude, longitude, name) {
				uni.openLocation({
					latitude: Number(latitude), //要去的纬度-地址       
					longitude: Number(longitude), //要去的经度-地址
					name: name, //地址名称
					address: name, //详细地址名称
					success: function() {
						console.log('导航成功');
					},
					fail: function(error) {
						console.log(error)
					}
				});
			},
			// 拨打电话
			bindphone(phone) {
				let that = this

				uni.showModal({
					title: '提示',
					content: '是否拨打电话',
					success: function(res) {
						if (res.confirm) {
							console.log('用户点击确定', that.phone);
							uni.makePhoneCall({
								phoneNumber: phone //仅为示例
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			binddetail(e) {
				console.log(e)
				uni.navigateTo({

					url: '/pages/index/game/order?id=' + e.orderTakingId
				})
			},
			getOrder() {
				let data = {
					id: this.id
				}
				this.$Request.get('/app/orders/queryOrders', data).then(res => {
					if (res.code == 0) {
						this.order = res.data
						if (this.order.appointInformation.tags) {
							this.order.appointInformation.tags = this.order.appointInformation.tags.split("|");
						}
						// if (this.order.startImg) {
						// 	this.order.startImg = this.order.startImg.split(",");
						// }
						// if (this.order.endImg) {
						// 	this.order.endImg = this.order.endImg.split(",");
						// }
					}
				})
			},
			delOrder(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '确定删除订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
							}
							that.$Request.get('/app/orders/deleteOrder', data).then(res => {
								if (res.code == 0) {
									uni.showToast({
										title: "删除成功"
									})
									// that.mescroll.resetUpScroll()
									uni.navigateBack()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
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
			},
			// 支付订单
			pay() {
				if(this.isPay==false){
					return
				}
				let that = this
				that.showPay = false;
				uni.showModal({
					title: '付款提示',
					content: '确认支付' + that.order.payMoney + '元吗?',
					success: function(re) {
						if (re.confirm) {
							that.isPay = true
							//classify  1app  2公众号 3小程序
							let classify = 1;
							if (that.openWay == 2) { //微信
								// #ifdef MP-WEIXIN
								that.$Request.post("/app/wxPay/wxPayOrder", {
									orderId: that.order.ordersId,
									classify: 3
								}).then(red => {
									if (red.code == 0) {
										uni.requestPayment({
											provider: 'wxpay',
											timeStamp: red.data.timestamp,
											nonceStr: red.data.noncestr,
											package: red.data.package,
											signType: red.data.signType,
											paySign: red.data.sign,
											success: function(redd) {
												uni.removeStorageSync('EditAddress')
												uni.showLoading({
													title: '支付成功'
												});
												that.isPay = true
												uni.hideLoading();
												setTimeout(function() {
													uni.navigateBack();
												}, 1000)
											},
											fail: function(err) {
												that.isPay = true
												uni.hideLoading();
												that.$queue.showToast(
													'支付失败');
											}
										});
									} else {
										that.isPay = true
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif

								// #ifdef H5
								let ua = navigator.userAgent.toLowerCase();
								console.log(ua)
								if (ua.indexOf('micromessenger') !== -1) {
									that.$Request.post("/app/wxPay/wxPayOrder", {
										orderId: that.order.ordersId,
										classify: 4
									}).then(red => {
										if (red.code == 0) {
											that.callPay(red.data);
										} else {
											that.isPay = true
											uni.showToast({
												title: red.msg,
												icon: 'none'
											})
										}
									});
								}

								// #endif

								// #ifdef APP-PLUS
								that.$Request.post("/app/wxPay/wxPayOrder", {
									orderId: that.order.ordersId,
									classify: 1
								}).then(red => {
									if (red.code == 0) {
										console.log(red,'+++++++++++++++++++++')
										that.setPayment('wxpay', JSON.stringify(red.data));
									} else {
										that.isPay = true
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif

							} else if (that.openWay == 1) { //支付宝
								// #ifdef H5
								that.$Request.post("/app/aliPay/payOrder", {
									orderId: that.order.ordersId,
									classify: 2
								}).then(red => {
									if (red.code == 0) {
										const div = document.createElement('div')
										div.innerHTML = red.data //此处form就是后台返回接收到的数据
										document.body.appendChild(div)
										document.forms[0].submit()
										that.isPay = true
									} else {
										that.isPay = true
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
								// #ifdef APP-PLUS
								that.$Request.post("/app/aliPay/payOrder", {
									orderId: that.order.ordersId,
									classify: 1
								}).then(red => {
									if (red.code == 0) {
										that.setPayment('alipay', red.data);
									} else {
										that.isPay = true
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
							} else if (that.openWay == 3) { //零钱
								that.$Request.post("/app/orders/payMoney", {
									ordersId: that.order.ordersId,
								}).then(res => {
									if (res.code == 0) {
										uni.showToast({
											title: '支付成功'
										})
										that.isPay = true
										setTimeout(function() {
											uni.navigateBack();
										}, 1000)
									} else {
										that.isPay = true
										uni.showToast({
											title: res.msg,
											icon: 'none'
										})
									}
								});
							}

						} else if (re.cancel) {
							that.isPay = true
							console.log('用户点击取消');
						}
					}

				})

			},
			// 完成订单
			cancel(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '订单完成后款项将支付给服务方，确认完成订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
								status: '2'
							}
							that.$Request.get('/app/orders/cancelOrder', data).then(res => {
								if (res.code == 0) {
									that.mescroll.resetUpScroll()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 取消订单
			cancelOrder(e) {
				let data = {
					id: e.ordersId,
					status: '3'
				}
				this.$Request.get('/app/orders/cancelOrder', data).then(res => {
					if (res.code == 0) {
						uni.showToast({
							title: '取消成功',
							icon: 'none'
						})
						setTimeout(function() {
							uni.navigateBack()
						}, 1000)
					}
				})
			},
			callPay: function(response) {
				if (typeof WeixinJSBridge === "undefined") {
					if (document.addEventListener) {
						document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady(response), false);
					} else if (document.attachEvent) {
						document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady(response));
						document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady(response));
					}
				} else {
					this.onBridgeReady(response);
				}
			},
			onBridgeReady: function(response) {
				let that = this;
				if (!response.package) {
					return;
				}
				WeixinJSBridge.invoke(
					'getBrandWCPayRequest', {
						"appId": response.appid, //公众号名称，由商户传入
						"timeStamp": response.timestamp, //时间戳，自1970年以来的秒数
						"nonceStr": response.noncestr, //随机串
						"package": response.package,
						"signType": response.signType, //微信签名方式：
						"paySign": response.sign //微信签名
					},
					function(res) {
						if (res.err_msg === "get_brand_wcpay_request:ok") {
							// 使用以上方式判断前端返回,微信团队郑重提示：
							//res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
							uni.removeStorageSync('EditAddress')
							uni.showLoading({
								title: '支付成功'
							});
							that.isPay = true
							uni.hideLoading();
							setTimeout(function() {
								uni.navigateBack();
							}, 1000)
						} else {
							that.isPay = true
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			selectWay: function(id) {
				this.openWay = id;
			},
			setPayment(name, order) {
				let that = this;
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.removeStorageSync('EditAddress')
						uni.showLoading({
							title: '支付成功'
						});
						that.isPay = true
						uni.hideLoading();
						setTimeout(function() {
							uni.navigateBack();
						}, 1000)
					},
					fail: function(err) {
						that.isPay = true
						uni.hideLoading();
						console.log(err,12)
					}
				});
			},
			goMsg() {
				let data = {
					userId: uni.getStorageSync('userId'),
					focusedUserId: this.order.user.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.user.userId == res.data.userId ? res.data.focusedUserId : this.order
							.user.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
		}
	}
</script>

<style>
	page {
		background: #f7f7f7;
	}

	.status {
		margin-top: 40rpx;
		color: #333333;
		font-size: 38rpx;
		font-weight: bold;
	}

	.types {
		width: 100%;
		height: 192rpx;
		background-color: #FFFFFF;
		border-radius: 16rpx;
		margin-top: 30rpx;
	}

	.types-box-r {
		/* width: 200upx; */
		text-align: right;
		margin-right: 30rpx;
		color: #FF2D01;
		font-size: 26rpx;
	}

	.types-box-r>text {
		font-size: 38rpx;
		font-weight: bold;
	}

	.types-box-r>span {
		font-size: 52rpx;
		font-weight: bold;
	}

	.types-r {
		margin-left: 30rpx;
	}

	.types-r-t {
		color: #333333;
		font-size: 32rpx;
		font-weight: bold;
	}

	.types-r-b {
		color: #999999;
		font-size: 26rpx;
		font-weight: 500;
		margin-top: 10rpx;
	}

	.bg {
		background: #FFFFFF;
	}

	.tabber {
		width: 100%;
		background: #ffffff;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 9;
		justify-content: flex-end;
		height: 127rpx;
		/* padding-right: 30rpx; */
	}

	.tit {
		overflow: hidden;
		text-overflow: ellipsis;
		word-wrap: break-word;
		white-space: normal;
		-webkit-line-clamp: 2;
		font-size: 30upx;
		/* font-weight: bold; */
		color: #a9a9a9;
	}

	.btn {
		background: #557EFD;
		color: #FFFFFF;
		border-radius: 50upx;
		padding: 24upx 0upx;
		text-align: center;
		width: 200upx;
		margin: 0 30upx 0upx 0;
	}

	.box {
		width: 100%;
		height: 100vh;
		background: rgba(0, 0, 0, 0.7);
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		z-index: 9999;
	}

	.bottbox {
		position: fixed;
		bottom: 0upx;
		left: 0;
		right: 0;
		z-index: 9999;
		background: #ffffff;
	}
</style>
