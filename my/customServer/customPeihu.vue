<template>
	<view class="content">
		<!-- 类型 -->
		<view class="types flex justify-center" @click="gotoNav(typeInfo)">
			<view class="types-box flex justify-center align-center">
				<view class="types-box-c flex justify-between align-center" style="border: none;">
					<view class="types-box-c-l flex align-center">
						<image src="../static/qcpz.png" mode=""></image>
						服务类型
					</view>
					<view class="types-box-c-r">
						{{typeInfo.serviceName}}
					</view>
				</view>
			</view>
		</view>
		<!-- 表单 -->
		<view class="types flex justify-center">
			<view class="types-box flex justify-center align-center flex-wrap">
				<view class="types-box-c flex justify-between align-center">
					<view @click="gotoHlr()" class="types-box-c-l flex align-center">
						被护理人
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.patient" type="text" disabled @click="gotoHlr()" input-align="right"
							placeholder="请选择被护理人" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<block v-if="emergencyPhone">
					<view class="types-box-c flex justify-between align-center">
						<view class="types-box-c-l flex align-center">
							紧急联系人
						</view>
						<view class="types-box-c-r flex align-center">
							<u-input v-model="emergencyPhone" type="text" disabled input-align="right"
								placeholder="请选择紧急联系人" />
							<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
						</view>
					</view>
				</block>

				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						服务医院
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.hospitalName" type="text" input-align="right" placeholder="请输入服务医院名称" />
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view @click="getAddress()" class="types-box-c-l flex align-center">
						医院地址
					</view>
					<view @click="getAddress()" class="types-box-c-r flex align-center">
						<u-input @click="getAddress()" v-model="from.detailsAddress" type="text" disabled
							input-align="right" placeholder="请选择医院地址" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						一级科室
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.departmentOneName" type="text" input-align="right"
							placeholder="请输入一级科室" />

					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						二级科室
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.departmentTwoName" type="text" input-align="right"
							placeholder="请输入二级科室" />

					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						床号
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.badNo" type="text" input-align="right" placeholder="请输入床号"
							style="margin-right: 30rpx;" />
						<!-- <u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon> -->
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view @click="showbz = true" class="types-box-c-l flex align-center">
						选择病症
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.symptom" type="text" disabled @click="showbz = true" input-align="right"
							placeholder="请选择病症" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view @click="showzl = true" class="types-box-c-l flex align-center">
						自理能力
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.selfAbility" type="text" disabled @click="showzl = true"
							input-align="right" placeholder="请选择自理能力" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view @click="showhl = true" class="types-box-c-l flex align-center">
						护理需求
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.nursingNeeds" disabled @click="showhl = true" type="text"
							input-align="right" placeholder="请选择护理需求" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view @click="show = true" class="types-box-c-l flex align-center">
						服务时间
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.hopeTime" @click="show = true" disabled type="text" input-align="right"
							placeholder="请选择服务时间" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						服务天数
					</view>
					<view class="types-box-c-r flex align-center">
						<!-- <u-input v-model="from.serviceNum" type="number" input-align="right" placeholder="请选择服务天数" />
						<u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon> -->
						<u-number-box :disabled-input="true" v-model="from.serviceNum" :min="1"></u-number-box>
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center">
					<view class="types-box-c-l flex align-center">
						联系电话
					</view>
					<view class="types-box-c-r flex align-center">
						<u-input v-model="from.phone" type="number" maxlength="11" input-align="right"
							placeholder="请输入联系电话" style="margin-right: 30rpx;" />
						<!-- <u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon> -->
					</view>
				</view>
				<view class="types-box-c flex justify-between align-center" @click="getyhqt">
					<view class="types-box-c-l flex align-center">
						优惠劵
					</view>
					<view class="types-box-c-r flex align-center">
						<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<!-- <view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view> -->
				<view class="" style="width: 626rpx;border-bottom: 1rpx solid #F2F2F2;">
					<view class="text-30 flex align-center"
						style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
						资料上传
						<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
					</view>
					<view class="img_remarks flex flex-wrap">
						<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
							<image :src="item" mode=""></image>
							<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close" color="red"
								size="38"></u-icon>
						</view>
						<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
							<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
							<view class="img_remarks-uploud-txt">
								图片上传
							</view>
						</view>

					</view>
				</view>
				<view class="types-box-c" style="height: auto !important;">
					<view class="types-box-c-l flex align-center" style="margin-top: 20rpx;">
						特殊需求
					</view>
					<view class="types-box-c-r" style="margin-top: 20rpx;">
						<u-input v-model="from.remarks" type="textarea" :height="height" placeholder="请输入其他服务需求..." />
						<!-- <u-icon name="arrow-right" color="#999999" style="margin-left: 10rpx;" size="28"></u-icon> -->
					</view>
				</view>
			</view>
		</view>
		<!-- 注意事项 -->
		<view class="zz flex justify-center">
			<view class="zz-box">
				<view class="zz-box-title">
					<u-icon name="error-circle-fill" color="#ffa722" style="margin-right: 10rpx;" size="32"></u-icon>
					注意事项
				</view>
				<view class="zz-box-con">
					{{typeInfo.mattersThing}}
				</view>
			</view>
		</view>
		<view class="flex justify-center"
			style="position: fixed;bottom: 0;background-color: #FFFFFF;width: 100%;padding-top: 30rpx;padding-bottom: 20rpx;">
			<view class="" style="width: 686rpx;">
				<view class="flex justify-center" style="width: 100%;margin-bottom: 20rpx;">

					<u-checkbox label-size="26rpx" v-model="checked" @change="checkboxChange" shape="circle">
						我已认真阅读预约相关<text @click.stop="gotoxieyi()" style="color: #4e86f8;">《服务条款同意书》</text></u-checkbox>
				</view>
				<view class=" flex justify-between" style="width: 100%;">
					<view style="color: #FF2D01;">
						实付：<text style="font-size: 38rpx;font-weight: bold;">￥</text>
						<text style="font-size: 52upx;margin-top: 8upx;font-weight: bold;">{{money}}</text>
					</view>
					<view class="">
						<u-button :custom-style="customStyle" @click="submit" shape="circle" :hair-line="false">立即支付
						</u-button>
					</view>
				</view>
			</view>
		</view>
		<!-- 协议弹窗 -->
		<u-popup v-model="showxy" mode="center" border-radius="24" width="80%" height="60%">
			<view class="" style="width: 100%;height: 100%;background-color: #FFFFFF;position: relative;">
				<view class="flex justify-center" style="width: 100%;height: 85%;">
					<view class="" style="width: 90%;height: 95%;margin-top: 5%;">
						<scroll-view scroll-y="true" style="width: 100%;height: 100%;background-color: #FFFFFF;">
							<view v-html="content"></view>
						</scroll-view>
					</view>
				</view>
				<view class="flex justify-center align-center"
					style="width: 100%;position: absolute;bottom: 0;background-color: #FFFFFF;height: 15%;">
					<view @click="showxy=false" class="flex justify-center align-center"
						style="width: 90%;height: 70rpx;border-radius: 40rpx;background-color: #4e86f8;color: #FFFFFF;">
						确认关闭
					</view>
				</view>
			</view>
		</u-popup>
		<!-- 服务时间选择 -->
		<u-picker v-model="show" :params="params" mode="time" @confirm="confirm"></u-picker>
		<!--自理能力 -->
		<u-select v-model="showzl" :list="list3" label-name="code" @confirm="confirmzl"></u-select>
		<!-- 病症選擇 -->
		<u-popup v-model="showbz" mode="bottom">
			<view class="bz flex justify-center">
				<view class="bz-box">
					<view class="bz-box-title">
						病症选择
					</view>
					<view class="bz-box-select flex justify-center align-center">
						<view class="bz-box-select-c flex flex-wrap justify-between">
							<view class="bz-box-select-item flex justify-center align-center" @click="select(item)"
								:class="item.se==true?'active':''" v-for="(item,index) in list4" :key="index">
								{{item.value}}
							</view>
							<view class="bz-box-select-item flex justify-center align-center"
								style="width: 200rpx;height: 0;">
							</view>
						</view>
					</view>
					<view class="bz-box-btn flex justify-center align-center" @click="setbz">
						确定
					</view>
				</view>
			</view>
		</u-popup>
		<!-- 护理需求 -->
		<u-popup v-model="showhl" mode="bottom">
			<view class="bz flex justify-center">
				<view class="bz-box">
					<view class="bz-box-title">
						护理需求
					</view>
					<view class="bz-box-select flex justify-center align-center">
						<view class="bz-box-select-c flex flex-wrap justify-between">
							<view class="bz-box-select-item flex justify-center align-center" @click="select(item)"
								:class="item.se==true?'active':''" v-for="(item,index) in list5" :key="index">
								{{item.value}}
							</view>
							<view class="bz-box-select-item flex justify-center align-center"
								style="width: 200rpx;height: 0;">
							</view>
						</view>
					</view>
					<view class="bz-box-btn flex justify-center align-center" @click="sethl">
						确定
					</view>
				</view>
			</view>
		</u-popup>
		<!-- 支付方式选择 -->
		<u-popup v-model="showPay" mode="bottom" border-radius="14" :closeable="true">
			<view
				style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
				选择支付方式
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
		</u-popup>
		<!-- 优惠券 -->
		<u-popup v-model="showYouhuijuan" mode="bottom" border-radius="14" height="900rpx" :closeable="true">
			<view style="padding-bottom: 30rpx;">
				<view
					style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;padding-top:15rpx;padding-bottom:40rpx;">
					优惠劵
				</view>
				<view class="margin-right text-right" @click="qingkong">
					<view class="noyouhui">不使用优惠劵</view>
				</view>
				<view class="listbox" v-for="(item,ind) in youhuiList" :key="ind">
					<view class="flex align-start justify-between padding-lr">
						<view>
							<view style="color: #000000;font-size: 30upx;">{{item.couponName}}</view>
							<view style="color: #999999;font-size: 24upx;margin-top: 10upx;" v-if="item.expirationTime">
								有效期至{{item.expirationTime}}
							</view>
							<view style="color: #999999;font-size: 24upx;margin-top: 10upx;" v-else>永久有效
							</view>
						</view>
						<view style="color: #FD3C44;font-size: 30upx;">¥<text
								style="font-size: 48upx;font-weight: bold;">{{item.money}}</text></view>
					</view>
					<view style="width: 100%;border-top:1rpx dashed #E6E6E6;margin: 30upx 0upx;"></view>
					<view class="flex align-center justify-between padding-lr">
						<view v-if="item.minMoney">满{{item.minMoney}}元可用</view>
						<view v-else>无门槛使用</view>
						<view class="btnss" @click="youhuiPay(item)">立即使用</view>
					</view>
				</view>
			</view>
		</u-popup>
	</view>
</template>

<script>
	import configdata from '../../common/config.js'
	export default {
		data() {
			return {
				imgRemarks: [], //图片备注
				isPay: true,
				checked: false,
				customStyle: {
					width: '340upx',
					color: '#FFFFFF',
					background: "#468EF8",
					border: 0
				},
				showxy: false,
				content: '',
				openWay: 0,
				showPay: false,
				openLists: [],
				money: 0,
				moneys: 0,
				showzl: false,
				showbz: false,
				showhl: false,
				list: [],
				list3: [],
				list4: [],
				list5: [],
				couponName: '',
				order: {},
				params: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: false,
					second: false
				},
				show: false,
				height: 200,
				from: {
					couponId: '', //优惠券id
					youhuiMoney: 0,
					serviceId: '', //服务id
					type: 1, // 1:陪护 2：陪诊
					patient: '', //被护理人名称
					patientId: '', //被护理人id
					hospitalName: '', //医院名称
					departmentOneName: '', //一级科室名称
					departmentTwoName: '', //二级科室名称
					lng: '', // 经度
					lat: '', // 纬度
					province: '', // 省
					city: '', // 市
					district: '', // 区
					detailsAddress: '', // 详细地址
					badNo: '', //床号
					symptom: '', //症状
					selfAbility: '', //自理能力
					nursingNeeds: '', //护理需求
					hopeTime: '', //服务时间
					serviceNum: 1, //服务天数
					phone: '', //联系电话
					remarks: '', //特殊需求
					orderTakingUserId: '', //护工id
				},
				czSel: '否',
				youhuiList: [],
				showYouhuijuan: false,
				typeInfo: {},
				emergencyPhone: '', //紧急联系人
			};
		},
		watch: {
			'from.serviceNum': function(newName, oldName) {
				console.log(this.from.youhuiMoney)
				if (this.moneys * parseInt(this.from.serviceNum) > parseFloat(this.from.youhuiMoney)) {
					this.money = this.moneys * parseInt(this.from.serviceNum) - parseFloat(this.from.youhuiMoney)
				} else {
					this.money = 0.01
				}

			},
			'from.youhuiMoney': function(newName, oldName) {
				if (this.moneys * parseInt(this.from.serviceNum) > parseFloat(this.from.youhuiMoney)) {
					this.money = this.moneys * parseInt(this.from.serviceNum) - parseFloat(this.from.youhuiMoney)
				} else {
					this.money = 0.01
				}
			},
			deep: true
		},
		onLoad(option) {
			this.czSel = this.$queue.getData('czSel');
			this.from.phone = uni.getStorageSync('phone')
			this.from.orderTakingUserId = option.hguserId
			this.typeInfo = JSON.parse(decodeURIComponent(option.info))
			// #ifdef APP-PLUS
			this.openLists = [{
				image: '/static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '/static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '/static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '/static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '/static/images/lingqian.png',
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
		},
		onShow() {
			let that = this
			//获取陪护服务id
			if (uni.getStorageSync('phserviceId')) {
				this.from.serviceId = uni.getStorageSync('phserviceId')
				this.getInfo(this.from.serviceId)
			}
			//获取选择的护理人信息
			uni.$on('updateData', (data) => {
				that.from.patient = data.realName
				that.from.patientId = data.patientId
				if (data.emergencyPhone) {
					that.emergencyPhone = data.emergencyPhone
				} else {
					that.emergencyPhone = ''
				}

			})
			//自理能力
			this.getZlList();
			//病症
			this.getBzList();
			//护理需求
			this.getHlList();
			//优惠券列表
			this.getMyList()
			//服务协议
			this.getxy()
		},
		methods: {
			//根据经纬度获取省市区
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						console.log(res.data.result.ad_info)
						this.from.province = res.data.result.ad_info.province // 省
						this.from.city = res.data.result.ad_info.city // 市
						this.from.district = res.data.result.ad_info.district // 区
					}
				});
			},
			//选择医院地址
			getAddress() {
				let that = this
				uni.chooseLocation({
					success(res) {
						console.log(res, '1111111')
						that.from.lat = res.latitude
						that.from.lng = res.longitude
						that.from.detailsAddress = res.name
						that.selectCity(that.from.lng, that.from.lat)
					},
					fail() {
						uni.showToast({
							title: '拉起地图失败，请重试!',
							icon: 'none'
						})
					}
				})
			},
			uploudImg() {
				let that = this
				//选择图片
				uni.chooseImage({
					sourceType: ['camera', 'album'],
					sizeType: 'compressed',
					count: 1,
					success: res => {
						//循环上传图片
						for (let i = 0; i < res.tempFilePaths.length; i++) {
							that.$queue.showLoading("上传中...");
							uni.uploadFile({ // 上传接口
								url: configdata.APIHOST1 + '/alioss/upload', //真实的接口地址
								// url: 'https://peizhensf.xianmaxiong.com/sqx_fast/alioss/upload',
								filePath: res.tempFilePaths[i],
								name: 'file',
								success: (uploadFileRes) => {
									that.imgRemarks.push(JSON.parse(uploadFileRes.data).data)
									uni.hideLoading();
								}
							});
						}
					}
				});
			},
			closeImg(index) {
				this.imgRemarks.splice(index, 1)
			},
			getyhqt() {
				if (this.youhuiList.length == 0) {
					uni.showToast({
						title: '暂无可用优惠券',
						icon: 'none'
					})
				} else {
					this.showYouhuijuan = true
				}
			},
			gotoNav(item) {
				uni.navigateTo({
					url: '/my/yxpeizhen/info?data=' + encodeURIComponent(JSON.stringify(item)) + '&type=1',

				})
			},
			//协议
			getxy() {
				this.$Request.get('/app/common/type/314').then(res => {
					if (res.code == 0) {
						this.content = res.data.value
					}
				});
			},
			//服务协议
			gotoxieyi() {
				uni.navigateTo({
					url: '/my/xieyi/xieyi?type=2'
				})
			},
			checkboxChange() {
				console.log(this.checked)
				if (this.checked == false) {
					this.showxy = true
				} else {
					this.showxy = false
				}
			},
			qingkong() {
				this.from.youhuiMoney = 0
				this.from.couponId = ''
				this.couponName = ''
				this.showYouhuijuan = false
			},
			youhuiPay(e) {

				if (Number(this.money) < Number(e.minMoney)) {
					uni.showToast({
						title: '使用优惠劵，下单金额必须大于' + e.minMoney + '元',
						icon: 'none'
					})
					return
				}
				this.from.youhuiMoney = e.money
				this.from.couponId = e.id
				this.couponName = e.money
				this.showYouhuijuan = false

			},
			//优惠券列表
			getMyList() {
				let data = {
					status: 0
				}
				this.$Request.getT('/app/couponUser/getMyCouponList', data).then(res => {
					if (res.code == 0) {
						this.youhuiList = res.data.records
					}
				});
			},
			//选择支付方式
			selectWay: function(id) {
				this.openWay = id;
			},
			//获取服务详情
			getInfo(serviceId) {
				let data = {
					serviceId: serviceId
				}
				this.$Request.getT("/app/hospitalEmploy/getHospitalEmployInfo", data).then(res => {
					if (res.code == 0) {
						this.order = res.data
						this.money = (res.data.money).toFixed(2)
						this.moneys = (res.data.money).toFixed(2)
					}
				})
			},
			pays() {
				if (this.isPay == false) {
					return
				}
				let that = this
				let data = this.from
				if (this.imgRemarks.length > 0) {
					data.imgRemarks = this.imgRemarks.join(',')
				} else {
					data.imgRemarks = ''
				}
				data.hopeTime = data.hopeTimes
				data.orderType = 2
				this.isPay = false
				that.$Request.getT("/app/orders/generateOrder", data).then(res => {
					if (res.code == 0) {
						that.showPay = false;
						uni.showModal({
							title: '付款提示',
							content: '确认支付' + that.money + '元吗?',
							complete: function(re) {
								uni.showLoading({
									title: '加载中...'
								})
								if (re.confirm) {
									let classify = 1;
									if (that.openWay == 2) { //微信
										// #ifdef MP-WEIXIN
										that.$Request.post("/app/wxPay/wxPayOrder", {
											orderId: res.data.ordersId,
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
														uni.hideLoading();
														uni.showToast({
															title: '支付成功'
														})
														that.isPay = true
														setTimeout(function() {
															uni.switchTab({
																url: '/pages/order/index'
															})

														}, 1000)
														uni.removeStorageSync(
															'carlist')
													},
													fail: function(err) {
														uni.hideLoading();
														that.isPay = true
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
												orderId: res.data.ordersId,
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
											orderId: res.data.ordersId,
											classify: 1
										}).then(red => {
											if (red.code == 0) {
												console.log(JSON.stringify(red))
												that.setPayment('wxpay', JSON.stringify(red
													.data));
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
											orderId: res.data.ordersId,
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
											orderId: res.data.ordersId,
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
											ordersId: res.data.ordersId,
										}).then(res => {
											if (res.code == 0) {
												uni.showToast({
													title: '支付成功'
												})
												that.isPay = true
												setTimeout(function() {
													uni.switchTab({
														url: '/pages/order/index'
													})

												}, 1000)
												uni.removeStorageSync('EditAddress')
											} else {
												that.isPay = true
												uni.showToast({
													title: res.msg,
													icon: 'none'
												})
											}
										});
									}
								} else {
									uni.hideLoading();
									uni.showToast({
										title: '已取消',
										icon: 'none'
									})
									that.isPay = true
									setTimeout(function() {
										uni.switchTab({
											url: '/pages/order/index'
										})

									}, 1000)
								}
							}
						})
					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
						that.isPay = true
					}

				});
			},
			pay() {
				let that = this
				that.pays()

			},
			//提交订单
			submit() {
				if (!this.from.patientId) {
					uni.showToast({
						title: '请选择被护理人',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.hospitalName) {
					uni.showToast({
						title: '请输入医院名称',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.detailsAddress) {
					uni.showToast({
						title: '请选择医院地址',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.departmentOneName) {
					uni.showToast({
						title: '请输入一级科室',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.departmentTwoName) {
					uni.showToast({
						title: '请输入一级科室',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.badNo) {
					uni.showToast({
						title: '请输入床号',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.symptom) {
					uni.showToast({
						title: '请选择症状',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.selfAbility) {
					uni.showToast({
						title: '请选择自理能力',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.nursingNeeds) {
					uni.showToast({
						title: '请选择护理需求',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.hopeTime) {
					uni.showToast({
						title: '请选择护服务时间',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.serviceNum) {
					uni.showToast({
						title: '请选择服务天数',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.from.phone) {
					uni.showToast({
						title: '请输入联系电话',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (this.checked == false) {
					uni.showToast({
						title: '请勾选服务条款同意书',
						icon: 'none',
						duration: 1000
					})
					return
				}

				this.showPay = true;
			},
			//设置护理需求
			sethl() {
				let arr = []
				this.list5.map(item => {
					if (item.se == true) {
						arr.push(item.value)
					}
				})
				this.from.nursingNeeds = arr.join(',')
				this.showhl = false
			},
			//护理需求列表
			getHlList() {
				let data = {
					type: '护理需求'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						res.data.map(item => {
							item.se = false
						})
						this.list5 = res.data
					}
				})
			},
			//设置病症
			setbz() {
				let arr = []
				this.list4.map(item => {
					if (item.se == true) {
						arr.push(item.value)
					}
				})
				this.from.symptom = arr.join(',')
				this.showbz = false
			},
			//选择病症
			select(item) {
				item.se = !item.se
			},
			//病症列表
			getBzList() {
				let data = {
					type: '病症'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						res.data.map(item => {
							item.se = false
						})
						this.list4 = res.data
					}
				})
			},
			//自理能力選擇
			confirmzl(e) {
				this.from.selfAbility = e[0].value
			},

			//自理能力列表
			getZlList() {
				let data = {
					type: '自理能力'
				}

				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						// let arr = JSON.parse(JSON.stringify(res.data).replace(/code/g, 'label'))
						this.list3 = res.data
						// console.log(arr)
					}
				})
			},
			gotoHlr() {
				uni.navigateTo({
					url: '/my/other/car?type=1'
				})
			},
			//获取当前系统时间
			getNowTime(tempminit) {
				if (!tempminit) {
					tempminit = 0;
				}
				var date = new Date();
				date.setMinutes(date.getMinutes() - tempminit);
				var year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second;
				/* console.log(timer); */
				return timer;
			},
			//比较时间大小
			dateCompare(startStr, endStr) {
				var d1, d2, s, arr, arr1, arr2;
				if (startStr.length > 10) {
					arr = startStr.split(" ");
					arr1 = arr[0].split("-");
					arr2 = arr[1].split(":");
					d1 = new Date(arr1[0], arr1[1] - 1, arr1[2], arr2[0], arr2[1], arr2[2]);
				} else {
					arr = startStr.split("-");
					d1 = new Date(arr[0], arr[1], arr[2]);
				}
				if (endStr.length > 10) {
					arr = endStr.split(" ");
					arr1 = arr[0].split("-");
					arr2 = arr[1].split(":");
					d2 = new Date(arr1[0], arr1[1] - 1, arr1[2], arr2[0], arr2[1], arr2[2]);
				} else {
					arr = endStr.split("-");
					d2 = new Date(arr[0], arr[1], arr[2]);
				}

				s = d2 - d1;
				if (s < 0) {
					return false;
				}
				return true;
			},
			//服务时间选择
			confirm(e) {

				//选中的时间
				let hopeTimes = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':00:00'
				//获取当前时间
				let tadayDate = this.getNowTime()
				let flag = this.dateCompare(tadayDate, hopeTimes)
				if (flag == true) { //开始时间小于当前时间
					this.from.hopeTimes = hopeTimes
					this.from.hopeTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + '时'
				} else {
					uni.showToast({
						title: '服务时间必须大于当前时间',
						icon: "none"
					})
					return
				}
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
							uni.hideLoading();
							uni.showToast({
								title: '支付成功'
							})
							that.isPay = true
							setTimeout(function() {
								uni.switchTab({
									url: '/pages/order/index'
								})

							}, 1000)
							uni.removeStorageSync('carlist')
						} else {
							that.isPay = true
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			setPayment(name, order) {
				let that = this;
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.hideLoading();
						uni.showToast({
							title: '支付成功'
						})
						that.isPay = true
						setTimeout(function() {
							uni.switchTab({
								url: '/pages/order/index'
							})

						}, 1000)
						uni.removeStorageSync('carlist')
					},
					fail: function(err) {
						that.isPay = true
						uni.hideLoading();
						console.log(12)
					}
				});
			}
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
	}

	.img_remarks {
		width: 100%;
		/* height: 100rpx; */
		/* background-color: red; */
		margin-top: 20rpx;
	}

	.img_remarks-uploud {
		width: 200rpx;
		height: 200rpx;
		border-radius: 10rpx;
		border: 1px dashed grey;
		text-align: center;
	}

	.img_remarks-item {
		width: 195rpx;
		height: 195rpx;
		border-radius: 10rpx;
		margin-right: 10rpx;
		margin-bottom: 10rpx;
		position: relative;

	}

	.close-circle-close {
		position: absolute;
		right: -10rpx;
		top: -10rpx;

	}

	.img_remarks-item>image {
		width: 100%;
		height: 100%;
		border-radius: 10rpx;
	}

	.img_remarks-uploud-icon {
		margin-top: 25%;
	}

	.img_remarks-uploud-txt {
		color: grey;
		width: 100%;
		text-align: center;
		margin-top: 10rpx;
	}

	.zz {
		width: 100%;
		// height: 200rpx;
		margin-top: 30rpx;
	}

	.zz-box {
		width: 686rpx;
		height: 100%;
	}

	.zz-box-title {
		width: 100%;
		font-size: 30rpx;
		font-weight: bold;
		padding-left: 30rpx;
		padding-right: 30rpx;
	}

	.zz-box-con {
		width: 100%;
		margin-top: 30rpx;
		padding-left: 30rpx;
		padding-right: 30rpx;
		font-size: 26rpx;
		color: #999999;
		letter-spacing: 2px;
	}

	.noyouhui {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 15upx;
		display: inline-block;
		padding: 5rpx 20rpx;
	}

	.listbox {
		/* background: #FFFFFF; */
		background: #F5F5F5;
		margin: 30rpx 30rpx;
		border-radius: 24rpx;
		padding: 30rpx 0;
	}

	.btnss {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 55upx;
		padding: 8upx 25upx;
	}

	.active {
		background-color: #468EF8 !important;
		color: #ffffff !important;
	}

	.bz {
		width: 100%;

		.bz-box {
			width: 686rpx;
			padding-bottom: 40rpx;

			.bz-box-title {
				width: 100%;
				text-align: center;
				font-weight: bold;
				margin-top: 20rpx;
			}

			.bz-box-select {
				width: 100%;
				margin-top: 20rpx;
				height: auto;

				.bz-box-select-c {
					width: 626rpx;
					height: 100%;

					.bz-box-select-item {
						width: 200rpx;
						height: 60rpx;
						background-color: #e6ebfe;
						border-radius: 40rpx;
						margin-bottom: 20rpx;
					}
				}
			}

			.bz-box-btn {
				width: 100%;
				height: 80rpx;
				background-color: #468EF8;
				border-radius: 40rpx;
				color: #ffffff;
				font-weight: bold;
			}
		}
	}

	.content {
		padding-bottom: 300rpx;
	}

	.types {
		width: 100%;
		// height: 120rpx;
		margin-top: 30rpx;

		.types-box {
			width: 686rpx;
			height: 100%;
			border-radius: 16rpx;
			background-color: #ffffff;

			.types-box-c {
				width: 626rpx;
				height: 120rpx;
				border-bottom: 1rpx solid #F2F2F2;
			}

			.types-box-c-l {
				color: #333333;
				font-size: 30rpx;
				font-weight: bold;

				image {
					width: 50rpx;
					height: 44rpx;
					margin-right: 20rpx;
				}
			}

			.types-box-c-r {
				color: #333333;
				font-size: 30rpx;
				// font-weight: bold;
			}
		}
	}

	.btn {
		width: 100%;
		height: 100rpx;
		background-color: #FFFFFF;
		position: fixed;
		bottom: 0;

		.btn-box {
			width: 686rpx;
			height: 100%;

			.btn-box-l {
				font-size: 24rpx;
				color: #FF2D01;

				text {
					font-size: 38rpx;
					font-weight: bold;
				}

				span {
					font-size: 52rpx;
					font-weight: bold;
				}
			}

			.btn-box-r {
				width: 240rpx;
				height: 80rpx;
				border-radius: 40rpx;
				background-color: #468EF8;
				color: #FFFFFF;
				font-size: 28rpx;
				font-weight: bold;
			}
		}
	}
</style>