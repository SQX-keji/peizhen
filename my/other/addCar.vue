<template>
	<view>
		<!-- 表单 -->
		<view class="from">
			<view class="from-box">
				<view class="from-box-con">
					<view class="from-box-con-item">
						<view class="tite">就诊人是否已满18周岁</view>
						<view class="from-box-con-item-r">
							<u-radio-group v-model="value">
								<u-radio @change="radioChange" v-for="(item, index) in list" :key="index"
									:name="item.name" :disabled="item.disabled">
									{{item.name}}
								</u-radio>
							</u-radio-group>
						</view>
					</view>
					<view class="from-box-con-item" >
						<view class="tite">性别</view>
						<view class="from-box-con-item-r">
							<u-input placeholder="请选择就诊人性别" @click="show=true" v-model="sexTit" fontSize="12" inputAlign="right"
								:clearable="false" :disabled="true">
							</u-input>
							<u-icon name="arrow-right" color="#CCCCCC" size="28"></u-icon>
						</view>
					</view>
					<view class="from-box-con-item">
						<view class="tite">姓名</view>
						<view class="from-box-con-item-r">
							<u-input placeholder="请输入就诊人姓名" v-model="realName" style="margin-right: 25rpx;"
								fontSize="12" inputAlign="right" :clearable="false">
							</u-input>
						</view>
					</view>
					<view class="from-box-con-item">
						<view class="tite">电话</view>
						<view class="from-box-con-item-r">
							<u-input type="number" placeholder="请输入就诊电话" maxlength="11" v-model="phone" style="margin-right: 25rpx;" fontSize="12"
								inputAlign="right" :clearable="false">
							</u-input>
						</view>
					</view>
					<view class="from-box-con-item">
						<view class="tite">身份证</view>
						<view class="from-box-con-item-r">
							<u-input placeholder="请输入身份证" v-model="idNumber" style="margin-right: 25rpx;" fontSize="12"
								inputAlign="right" :clearable="false">
							</u-input>
						</view>
					</view>
					<view class="from-box-con-item" >
						<view class="tite">就诊人关系</view>
						<view class="from-box-con-item-r" >
							<u-input placeholder="请选择就诊关系" @click="shows = true" v-model="relationship" 
								fontSize="12" inputAlign="right" :clearable="false" :disabled="true">
							</u-input>
							<u-icon name="arrow-right" color="#CCCCCC" size="28"></u-icon>
						</view>
					</view>
					<view class="from-box-con-item" style="border: none;">
						<view class="tite">紧急联系人</view>
						<view class="from-box-con-item-r" style="width: 80%;">
							<u-input type="number" placeholder="请输入紧急联系人电话(选填)" maxlength="11" v-model="emergencyPhone" style="margin-right: 25rpx;" fontSize="12"
								inputAlign="right" :clearable="false">
							</u-input>
						</view>
					</view>

				</view>
			</view>
		</view>
		<u-select v-model="show" :list="listsex" @confirm="confirm"></u-select>

		<u-select v-model="shows" :list="relationshipList" @confirm="selConfirm"></u-select>

		<!-- 添加就诊人 -->
		<view class="addCar">
			<!-- <view class="addCar-box" @click="submit()" v-if="!carId">保存</view> -->
			<view class="addCar-box" @click="submit()">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				emergencyPhone:'',
				value: '是',
				isUnderAge: '1',
				list: [{
						id: 1,
						name: '是'
					},
					{
						id: 2,
						name: '否'
					}
				],

				show: false,
				listsex: [{
						value: '1',
						label: '男'
					},
					{
						value: '2',
						label: '女'
					}
				],
				sex: '',
				sexTit: '',
				realName: '',
				phone: '',
				idNumber: '',
				relationship: '',
				patientId: '',
				shows: false,
				relationshipList: []
			};
		},
		onLoad(option) {
			if (option.patientId) {
				this.patientId = option.patientId
				this.getcarDetal()
			}
			this.userId = this.$queue.getData("userId");
			this.getlist()
		},
		onShow() {

		},
		methods: {
			getlist() {
				let data = {
					type: '就诊人关系'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						let dictist = res.data
						var arr = []
						for (let i = 0; i < dictist.length; i++) {
							var data = {}
							data.label = dictist[i].value
							data.value = i
							arr.push(data)
						}
						// console.log(arr)
						this.relationshipList = arr
						// console.log(this.relationshipList)
					}
				})
			},
			selConfirm(e) {
				console.log(e, '--------', e[0].label)
				this.relationship = e[0].label
			},
			radioChange(e) {
				if (e == '是') {
					this.isUnderAge = 1
				} else if (e == '否') {
					this.isUnderAge = 2
				}
			},
			confirm(e) {
				console.log(e, '--------')
				this.sexTit = e[0].label
				this.sex = e[0].value
			},
			navgo() {
				uni.navigateTo({
					url: '/my/other/index'
				})
			},
			submit() {
				if (!this.sexTit) {
					uni.showToast({
						title: '请选择性别',
						icon: 'none'
					})
					return
				}

				if (!this.realName) {
					uni.showToast({
						title: '请输入就诊人姓名',
						icon: 'none'
					})
					return
				}
				if (!this.phone) {
					uni.showToast({
						title: '请输入就诊电话',
						icon: 'none'
					})
					return
				}
				if(this.phone.length != 11) {
 					uni.showToast({
 						title: '请输入正确就诊电话',
 						icon: 'none'
 					})
 					return
 				}
				if (!this.idNumber) {
					uni.showToast({
						title: '请输入身份证',
						icon: 'none'
					})
					return
				}
				if (!this.relationship) {
					uni.showToast({
						title: '请选择就诊关系',
						icon: 'none'
					})
					return
				}
				
				if (this.patientId) {
					let data = {
						isUnderAge: this.isUnderAge,
						sex: this.sex,
						realName: this.realName,
						phone: this.phone,
						idNumber: this.idNumber,
						relationship: this.relationship,
						patientId: this.patientId,
						emergencyPhone:this.emergencyPhone
					}
					this.$Request.post("/app/patientInfo/savePatient", data).then(res => {
						if (res.code == 0) {
							uni.showToast({
								title: '修改成功！',
								icon: 'none'
							})
							setTimeout(()=>{
								uni.navigateBack()
							},1500)
						}else{
							uni.showToast({
								title:res.msg,
								icon:'none'
								
							})
						}
					})
				} else {
					let data = {
						isUnderAge: this.isUnderAge,
						sex: this.sex,
						realName: this.realName,
						phone: this.phone,
						idNumber: this.idNumber,
						relationship: this.relationship,
						emergencyPhone:this.emergencyPhone
					}
					this.$Request.post("/app/patientInfo/savePatient", data).then(res => {
						if (res.code == 0) {
							uni.showToast({
								title: '添加成功！',
								icon: 'none'
							})
							setTimeout(()=>{
								uni.navigateBack()
							},1500)
							
						}else{
							uni.showToast({
								title:res.msg,
								icon:'none'
								
							})
						}
					})
				}

			},
			getcarDetal() {
				let data = {
					patientId: this.patientId
				}
				this.$Request.get("/app/patientInfo/getPatientInfo", data).then(res => {
					if (res.code == 0) {
						this.realName = res.data.realName
						this.phone = res.data.phone
						this.idNumber = res.data.idNumber
						this.relationship = res.data.relationship
						this.emergencyPhone = res.data.emergencyPhone
						if (res.data.isUnderAge == 1) {
							this.value = '是'
						} else if (res.data.isUnderAge == 2) {
							this.value = '否'
						}
						if (res.data.sex == 1) {
							this.sexTit = '男'
						} else if (res.data.sex == 2) {
							this.sexTit = '女'
						}


					}
				})
			}
		}
	}
</script>

<style lang="less">
	page {
		background-color: #F5F5F5;
	}

	.from {
		width: 100%;
		height: auto;
		display: flex;
		justify-content: center;
		margin-top: 20rpx;

		.from-box {
			width: 686rpx;
			height: 100%;
			border-radius: 24rpx;
			background-color: #ffffff;
			display: flex;
			justify-content: center;

			.from-box-con {
				width: 612rpx;
				height: 100%;
			}

			.from-box-con-item {
				width: 100%;
				height: 106rpx;
				border-bottom: 1rpx solid #F2F2F2;
				display: flex;
				justify-content: space-between;
				align-items: center;



				.from-box-con-item-r {
					display: flex;
					align-items: center;
					font-size: 28upx;
				}
			}
		}
	}

	.tite {
		font-size: 28rpx;
		color: #333333;
		font-weight: bold;
		width: 300upx;
	}

	.addCar {
		width: 100%;
		height: 150rpx;
		display: flex;
		justify-content: center;
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

	.u-input--border {
		border: none !important;
	}
</style>
