<template>
	<view style="padding-bottom: 140px;">
		<view class="padding">
			<view class="types flex align-center" @click="gotoNav(order)">
				<image :src="order.img?order.img:'../../../static/logo.png'"
					style="width: 74rpx;height: 66rpx;margin-left: 44rpx;" mode="">
				</image>
				<view class="types-r">
					<view class="types-r-t">{{order.serviceName}}</view>
					<view class="types-r-b flex align-center" v-if="order.tags && order.tags.length>0">
						<view v-for="(ite,ind) in order.tags" :key="ind">
							{{ite}} <text v-if="ind!=order.tags.length-1" class="padding-lr-xs">|</text>
						</view>
					</view>
				</view>
			</view>
			<view class="padding bg margin-top radius u-skeleton-fillet">
				<!-- 医院名称 -->
				<view class="flex align-center justify-between padding-tb-xs">
					<view class="text-30" style="color: #1E1F31;width: 200upx;font-weight: bold;">
						<text>医院</text>
					</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs ">
							<u-input v-model="hospitalName" type="text" placeholder="请输入医院名称"
								placeholder-style="color:#999999;font-size:13px;" />
						</view>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<!-- 医院地址 -->
				<view class="flex align-center justify-between padding-tb-xs">
					<view @click="getAddress()" class="text-30" style="color: #1E1F31;width: 200upx;font-weight: bold;">
						<text>医院地址</text>
					</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs ">
							<u-input v-model="detailsAddress" disabled @click="getAddress()" type="text"
								placeholder="请选择医院地址" placeholder-style="color:#999999;font-size:13px;" />

						</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<!-- 指定陪诊人 -->
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between padding-tb-xs" @click="getPeopList()">
					<view class="text-30" style="color: #1E1F31;width: 200upx;font-weight: bold;">
						<text>指定陪诊人</text>
					</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs ">
							<u-input v-model="orderTakingUserName" @click="getPeopList()" type="text" :disabled="true"
								placeholder="请选择陪诊员(选填)" placeholder-style="color:#999999;font-size:13px;" />
						</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between ">
					<view class="text-30" @click="totojzr()" style="color: #1E1F31;width: 240upx;font-weight: bold;">就诊人
					</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs ">
							<u-input v-model="patientName" :clearable="false" type="text" @click="totojzr()"
								:disabled="true" placeholder="请选择就诊人"
								placeholder-style="color:#999999;font-size:13px;" />
						</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<block v-if="emergencyPhone">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">紧急联系人</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="emergencyPhone" :clearable="false" type="text" :disabled="true"
									placeholder="请选择紧急联系人" placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
				</block>

				<block v-if="serviceType==1">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" @click="show = true"
							style="color: #1E1F31;width: 240upx;font-weight: bold;">期望就诊时间</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs">
								<u-input v-model="hopeTime" @click="show = true" :disabled="true" :clearable="false"
									type="text" placeholder="请选择就诊时间"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">一级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentOneName" :clearable="false" type="text"
									placeholder="请输入一级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">二级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentTwoName" :clearable="false" type="text"
									placeholder="请输入二级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">接送地址</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="address" :clearable="false" type="text" @click="gotoAddress()"
									:disabled="true" placeholder="请选择接送地址"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="getyhqt">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">优惠劵</view>
						<view class="flex justify-between padding-tb-sm">
							<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30 flex align-center"
							style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
							资料上传
							<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
						</view>
						<view class="img_remarks flex flex-wrap">
							<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
								<image :src="item" mode=""></image>
								<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close"
									color="red" size="38"></u-icon>
							</view>
							<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
								<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
								<view class="img_remarks-uploud-txt">
									图片上传
								</view>
							</view>

						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30" style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">备注说明</view>
						<textarea v-model="remarks" class="text-white text-df flex-sub padding-tb-sm "
							placeholder="请输入其他服务需求..." style="height: 200upx;"></textarea>
					</view>

				</block>
				<block v-if="serviceType==2 || serviceType==3 || serviceType==4">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" @click="show = true"
							style="color: #1E1F31;width: 240upx;font-weight: bold;">期望就诊时间</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs">
								<u-input v-model="hopeTime" @click="show = true" :disabled="true" :clearable="false"
									type="text" placeholder="请选择就诊时间"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">一级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentOneName" :clearable="false" type="text"
									placeholder="请输入一级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">二级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentTwoName" :clearable="false" type="text"
									placeholder="请输入二级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="getyhqt">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">优惠劵</view>
						<view class="flex justify-between padding-tb-sm">
							<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30 flex align-center"
							style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
							资料上传
							<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
						</view>
						<view class="img_remarks flex flex-wrap">
							<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
								<image :src="item" mode=""></image>
								<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close"
									color="red" size="38"></u-icon>
							</view>
							<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
								<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
								<view class="img_remarks-uploud-txt">
									图片上传
								</view>
							</view>

						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30" style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">备注说明</view>
						<textarea v-model="remarks" class="text-white text-df flex-sub padding-tb-sm "
							placeholder="请输入其他服务需求..." style="height: 200upx;"></textarea>
					</view>
				</block>
				<block v-if="serviceType==5">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">期望处理时间</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs">
								<u-input v-model="hopeTime" @click="show = true" :disabled="true" :clearable="false"
									type="text" placeholder="请选择期望处理时间"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">收件信息</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="address" :clearable="false" type="text" @click="gotoAddress()"
									:disabled="true" placeholder="请选择收件信息"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">报告信息</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="reportType" :clearable="false" type="text" @click="serviceshow = true"
									:disabled="true" placeholder="请选择报告信息"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="getyhqt">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">优惠劵</view>
						<view class="flex justify-between padding-tb-sm">
							<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30 flex align-center"
							style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
							资料上传
							<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
						</view>
						<view class="img_remarks flex flex-wrap">
							<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
								<image :src="item" mode=""></image>
								<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close"
									color="red" size="38"></u-icon>
							</view>
							<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
								<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
								<view class="img_remarks-uploud-txt">
									图片上传
								</view>
							</view>

						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30" style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">备注说明</view>
						<textarea v-model="remarks" class="text-white text-df flex-sub padding-tb-sm "
							placeholder="请输入其他服务需求..." style="height: 200upx;"></textarea>
					</view>
				</block>
				<block v-if="serviceType==6">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" @click="show = true"
							style="color: #1E1F31;width: 240upx;font-weight: bold;">期望就诊时间</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs">
								<u-input v-model="hopeTime" @click="show = true" :disabled="true" :clearable="false"
									type="text" placeholder="请选择就诊时间"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">一级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentOneName" :clearable="false" type="text"
									placeholder="请输入一级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">二级科室</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="departmentTwoName" :clearable="false" type="text"
									placeholder="请输入二级科室名称" placeholder-style="color:#999999;font-size:13px;" />
							</view>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="exclusiveTypeShow = true">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">专享归属</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs padding-tb-xs">
								<u-input v-model="exclusiveType" :clearable="false" :disabled="true"
									@click="exclusiveTypeShow = true" type="text" placeholder="请选择专享归属"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="getyhqt">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">优惠劵</view>
						<view class="flex justify-between padding-tb-sm">
							<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30 flex align-center"
							style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
							资料上传
							<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
						</view>
						<view class="img_remarks flex flex-wrap">
							<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
								<image :src="item" mode=""></image>
								<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close"
									color="red" size="38"></u-icon>
							</view>
							<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
								<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
								<view class="img_remarks-uploud-txt">
									图片上传
								</view>
							</view>

						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30" style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">备注说明</view>
						<textarea v-model="remarks" class="text-white text-df flex-sub padding-tb-sm "
							placeholder="请输入其他服务需求..." style="height: 200upx;"></textarea>
					</view>
				</block>
				<block v-if="serviceType==7">
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">药物类型</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="drugsType" :clearable="false" type="text"
									@click="drugsTypeshow = true" :disabled="true" placeholder="请选择药物类型"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">药物名称</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="drugsName" :clearable="false" type="text" placeholder="请准确输入药物名称"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">期望处理时间</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs">
								<u-input v-model="hopeTime" @click="show = true" :disabled="true" :clearable="false"
									type="text" placeholder="请选择期望处理时间"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between ">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">收件信息</view>
						<view class="flex justify-between ">
							<view class="margin-right-xs ">
								<u-input v-model="address" :clearable="false" type="text" @click="gotoAddress()"
									:disabled="true" placeholder="请选择请选择收件信息"
									placeholder-style="color:#999999;font-size:13px;" />
							</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="flex align-center justify-between " @click="getyhqt">
						<view class="text-30" style="color: #1E1F31;width: 240upx;font-weight: bold;">优惠劵</view>
						<view class="flex justify-between padding-tb-sm">
							<view class="margin-right-xs">{{-couponName?-couponName:'立即使用'}}</view>
							<u-icon name="arrow-right" color="#999999"></u-icon>
						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30 flex align-center"
							style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">
							资料上传
							<text style="font-size: 20rpx;">(就诊卡、病例等)</text>
						</view>
						<view class="img_remarks flex flex-wrap">
							<view class="img_remarks-item" v-for="(item,index) in imgRemarks" :key="index">
								<image :src="item" mode=""></image>
								<u-icon name="close-circle" @click="closeImg(index)" class="close-circle-close"
									color="red" size="38"></u-icon>
							</view>
							<view @click="uploudImg()" class="img_remarks-uploud" v-if="imgRemarks.length<9">
								<u-icon class="img_remarks-uploud-icon" name="camera" color="grey" size="48"></u-icon>
								<view class="img_remarks-uploud-txt">
									选择图片
								</view>
							</view>

						</view>
					</view>
					<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
					<view class="">
						<view class="text-30" style="color: #1E1F31;margin-right: 20upx;font-weight: bold;">备注说明</view>
						<textarea v-model="remarks" class="text-white text-df flex-sub padding-tb-sm "
							placeholder="请输入其他服务需求..." style="height: 200upx;"></textarea>
					</view>
				</block>
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
					{{order.mattersThing}}
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
						<text style="font-size: 52upx;margin-top: 8upx;font-weight: bold;">{{totalprice}}</text>
					</view>
					<view class="">
						<u-button :custom-style="customStyle" @click="openPay" shape="circle" :hair-line="false">立即支付
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
		<u-picker v-model="show" mode="time" :mask-close-able="false" :params="params" @confirm="statusChange">
		</u-picker>

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
						<view class="btn" @click="youhuiPay(item)">立即使用</view>
					</view>
				</view>
			</view>
		</u-popup>
		<!-- 报告类型 -->
		<u-select v-model="serviceshow" :list="reportTypeList" value-name="value" label-name="value"
			@confirm="confirmtype"></u-select>
		<!-- 归属专享 -->
		<u-action-sheet :list="exclusiveTypeList" v-model="exclusiveTypeShow" @click="click"></u-action-sheet>
		<!-- 药物类型 -->
		<u-select v-model="drugsTypeshow" :list="drugsTypeshowList" value-name="value" label-name="value"
			@confirm="confirmywu"></u-select>
	</view>
</template>

<script>
	import configdata from '../../common/config.js'
	export default {
		data() {
			return {
				imgRemarks: [], //图片备注
				isPay: true, //防抖
				exclusiveTypeShow: false,
				exclusiveType: '',
				serviceshow: false,
				showxy: false,
				content: '',
				loading: true, // 是否显示骨架屏组件
				customStyle: {
					width: '340upx',
					color: '#FFFFFF',
					background: "#468EF8",
					border: 0
				},
				checked: false,
				serviceId: '',
				order: {},
				price: '',
				show: false,
				params: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: false,
					second: false,
					timestamp: false
				},
				hopeTime: '',
				hopeTimes: '',
				startHour: '',
				showPay: false,
				openWay: 0,
				openLists: [],

				modularId: '',
				hospitalId: '', //医院id
				hospitalName: '', //医院名称
				orderTakingUserId: '', //护工id
				orderTakingUserName: '', //护工名称
				jiuzhenlist: [],
				patientId: '', //就诊人id
				patientName: '', //就诊人名称
				emergencyPhone: '', //紧急联系人
				departmentName: '',
				phone: '',
				remarks: '',
				serviceNum: 1, //服务天数
				czSel: '否',
				showYouhuijuan: false,
				youhuiList: [],
				couponId: '',
				couponName: '',
				youhuiMoney: 0,
				page: 1,
				limit: 10,
				totalprice: 0,
				serviceType: '',
				hospitalInfo: {},
				reportType: '', //报告类型
				reportTypeList: [], //报告类型数组
				addressId: '', //地址id
				address: '', //地址位置
				exclusiveTypeList: [],
				drugsType: '', //药物类型
				drugsName: '', //药物名称
				drugsTypeshow: false,
				drugsTypeshowList: [],
				departmentOneName: '', //一级科室名称
				departmentTwoName: '', //二级科室名称
				lng: '', // 经度
				lat: '', // 纬度
				province: '', // 省
				city: '', // 市
				district: '', // 区
				detailsAddress: '', // 详细地址


			}
		},
		onLoad(option) {
			if (option.orderTakingUserName && option.orderTakingUserId) {
				this.orderTakingUserName = option.orderTakingUserName
				this.orderTakingUserId = option.orderTakingUserId
			}
			this.czSel = this.$queue.getData('czSel');
			this.serviceType = option.serviceType
			if (this.serviceType == 5) {
				this.getreportType()
			}
			if (this.serviceType == 6) {
				this.getzxgs()
			}
			if (this.serviceType == 7) {
				this.getdrugsType()
			}
			this.serviceId = option.serviceId
			uni.removeStorageSync('jiuzhenlist')

			this.getDet()
			this.getMyList();
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
		destryed() {
			// uni.removeStorageSync('jiuzhenlist')
		},
		onShow() {
			let that = this
			uni.$on('peizhenPeople', (data) => {
				// console.log(data, 'sssssssss')
				that.orderTakingUserId = data.orderTakingUserId;
				that.orderTakingUserName = data.orderTakingUserName;
			})
			uni.$on('jiuzhenlist', (data) => {
				// console.log(data, 'sssssssss')
				that.patientId = data.patientId
				that.patientName = data.realName
				if (data.emergencyPhone) {
					that.emergencyPhone = data.emergencyPhone
				} else {
					that.emergencyPhone = ''
				}

			})
			this.getxy();
			uni.$on('address', (data) => {
				that.addressId = data.addressId
				that.address = data.province + '' + data.city + '' + data.district + ' ' + data.address
			})
		},
		methods: {
			//根据经纬度获取省市区
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						console.log(res.data.result.ad_info)
						this.province = res.data.result.ad_info.province // 省
						this.city = res.data.result.ad_info.city // 市
						this.district = res.data.result.ad_info.district // 区
					}
				});
			},
			//选择医院地址
			getAddress() {
				let that = this
				uni.chooseLocation({
					success(res) {
						console.log(res, '1111111')
						that.lat = res.latitude
						that.lng = res.longitude
						that.detailsAddress = res.name
						that.selectCity(that.lng, that.lat)
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
			//选择护工
			getPeopList() {
				uni.navigateTo({
					url: '/my/peizhenPeople/peizhenPeople'
				})
			},
			hospitalInfock(hospitalId) {
				uni.navigateTo({
					url: '/my/hospital/hospital?hospitalId=' + hospitalId
				})
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
			confirmywu(e) {
				this.drugsType = e[0].value
			},
			//获取药物类型
			getdrugsType() {

				let data = {
					type: '药物类型'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						this.drugsTypeshowList = res.data
					}
				})
			},
			click(index) {
				this.exclusiveType = this.exclusiveTypeList[index].text
			},
			//专享归属
			getzxgs() {
				let data = {
					type: '专享归属'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						res.data.map(item => {
							item.text = item.value
						})
						this.exclusiveTypeList = res.data
					}
				})
			},
			//去地址管理
			gotoAddress() {
				uni.navigateTo({
					url: '/my/address/address?type=1'
				})
			},
			confirmtype(e) {
				this.reportType = e[0].value
			},
			//获取报告类型
			getreportType() {
				let data = {
					type: '报告类型'
				}
				this.$Request.get("/app/dict/selectDictList", data).then(res => {
					if (res.code == 0) {
						this.reportTypeList = res.data
					}
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
			//协议
			getxy() {
				this.$Request.get('/app/common/type/313').then(res => {
					if (res.code == 0) {
						this.content = res.data.value
					}
				});
			},
			//服务协议
			gotoxieyi() {
				uni.navigateTo({
					url: '/my/xieyi/xieyi?type=1'
				})
			},
			valChange(e) {
				// console.log('当前值为: ' + e.value)
				if (e.value == 0) {
					uni.showToast({
						title: '服务天数不能低于1天',
						icon: 'none'
					})
					return
				} else {
					if (!this.couponId) {
						this.totalprice = parseFloat(this.price * e.value).toFixed(2)
					} else {
						let totalprice = parseFloat(this.price * e.value).toFixed(2)
						if (parseFloat(totalprice) > parseFloat(this.couponName)) {
							this.totalprice = parseFloat(totalprice - this.couponName).toFixed(2)
						} else {
							this.totalprice = 0.01
						}

					}
				}
			},
			qingkong() {
				this.couponId = ''
				this.couponName = ''
				// alert(this.price+'-'+this.serviceNum)
				this.totalprice = parseFloat(this.price * this.serviceNum).toFixed(2)
				this.showYouhuijuan = false
			},
			youhuiPay(e) {
				let price
				if (this.serviceNum != 0) {
					price = this.price * this.serviceNum
				} else {
					price = this.price
				}
				console.log(price, price >= e.minMoney)
				// return
				if (Number(price) >= Number(e.minMoney)) {
					// let totalprice = parseFloat(this.order.money * this.number).toFixed(0)
					if (parseFloat(price) > parseFloat(e.money)) {
						this.totalprice = parseFloat(price - e.money).toFixed(2)
					} else {
						this.totalprice = 0.01
					}

					console.log(this.totalprice, parseFloat(price - e.money))
				} else {
					uni.showToast({
						title: '使用优惠劵，下单金额必须大于' + e.minMoney + '元',
						icon: 'none'
					})
					return
				}

				this.youhuiMoney = e.money
				this.openWay = 3
				this.couponId = e.id
				this.couponName = e.money
				this.showYouhuijuan = false

			},
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
			//就诊人
			totojzr() {
				uni.navigateTo({
					url: '/my/other/car'
				})
			},
			selectWay: function(id) {
				this.openWay = id;
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
			statusChange(e) {
				console.log(e)
				//选中的时间
				let hopeTimes = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':00:00'
				//获取当前时间
				let tadayDate = this.getNowTime()
				let flag = this.dateCompare(tadayDate, hopeTimes)
				if (flag == true) { //开始时间小于当前时间
					this.hopeTimes = hopeTimes
					this.hopeTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + '时'
				} else {
					uni.showToast({
						title: '期望就诊时间必须大于当前时间',
						icon: "none"
					})
					return
				}
			},
			// 详情
			getDet() {
				this.$Request.get("/app/hospitalEmploy/getHospitalEmployInfo", {
					serviceId: this.serviceId
				}).then(res => {
					if (res.code == 0) {
						this.order = res.data
						if (this.order.tags) {
							this.order.tags = res.data.tags.split(',')
						}
						this.price = res.data.money
						this.totalprice = (res.data.money).toFixed(2)
						this.loading = false;
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			openKeshi() {
				if (this.hospitalId) {
					uni.navigateTo({
						url: '/my/keshi/index?hospitalId=' + this.hospitalId
					})
				} else {
					uni.showToast({
						title: '请选择医院',
						icon: 'none'
					})
				}

			},
			openPay() {
				if (!this.hospitalName) {
					uni.showToast({
						title: '请输入医院名称',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.detailsAddress) {
					uni.showToast({
						title: '请选择医院地址',
						icon: 'none',
						duration: 1000
					})
					return
				}

				if (this.serviceType == 1) {
					if (!this.patientName) {
						uni.showToast({
							title: '请选择就诊人',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.hopeTime) {
						uni.showToast({
							title: '请选择期望就诊时间',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentOneName) {
						uni.showToast({
							title: '请输入一级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentTwoName) {
						uni.showToast({
							title: '请输入二级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.addressId) {
						uni.showToast({
							title: '请选择接送地址',
							icon: 'none',
							duration: 1000
						})
						return
					}
				}
				if (this.serviceType == 2 || this.serviceType == 3 || this.serviceType == 4) {
					if (!this.patientName) {
						uni.showToast({
							title: '请选择就诊人',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.hopeTime) {
						uni.showToast({
							title: '请选择期望就诊时间',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentOneName) {
						uni.showToast({
							title: '请输入一级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentTwoName) {
						uni.showToast({
							title: '请输入二级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
				}
				if (this.serviceType == 5) {
					if (!this.patientName) {
						uni.showToast({
							title: '请选择就诊人',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.hopeTime) {
						uni.showToast({
							title: '请选择期望处理时间',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.addressId) {
						uni.showToast({
							title: '请选择收件信息',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.reportType) {
						uni.showToast({
							title: '请选择报告类型',
							icon: 'none',
							duration: 1000
						})
						return
					}
				}
				if (this.serviceType == 6) {
					if (!this.patientName) {
						uni.showToast({
							title: '请选择就诊人',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.hopeTime) {
						uni.showToast({
							title: '请选择期望就诊时间',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentOneName) {
						uni.showToast({
							title: '请输入一级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.departmentTwoName) {
						uni.showToast({
							title: '请输入二级科室名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.exclusiveType) {
						uni.showToast({
							title: '请选择专享归属',
							icon: 'none',
							duration: 1000
						})
						return
					}

				}
				if (this.serviceType == 7) {
					if (!this.patientName) {
						uni.showToast({
							title: '请选择就诊人',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.drugsType) {
						uni.showToast({
							title: '请选择药物类型',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.drugsName) {
						uni.showToast({
							title: '请准确输入药物名称',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.hopeTime) {
						uni.showToast({
							title: '请选择期望处理时间',
							icon: 'none',
							duration: 1000
						})
						return
					}
					if (!this.addressId) {
						uni.showToast({
							title: '请选择收件信息',
							icon: 'none',
							duration: 1000
						})
						return
					}

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
			pays() {
				if (this.isPay == false) {
					return
				}
				let that = this
				this.isPay = false
				that.$Request.get("/app/orders/generateOrder", {
					serviceId: that.serviceId,
					type: 1,
					hospitalName: this.hospitalName, //医院名称
					lng: this.lng, //经度
					lat: this.lat, //纬度
					province: this.province, //省
					city: this.city, //市
					district: this.district, //区
					detailsAddress: this.detailsAddress, //详细地址
					patientId: that.patientId,
					hopeTime: that.hopeTimes,
					departmentOneName: this.departmentOneName, //一级科室名称
					departmentTwoName: this.departmentTwoName, //二级级科室名称
					remarks: that.remarks,
					phone: that.phone,
					serviceNum: that.serviceNum,
					couponId: that.couponId,
					addressId: that.addressId,
					reportType: that.reportType,
					exclusiveType: that.exclusiveType,
					drugsType: that.drugsType,
					drugsName: that.drugsName,
					serviceType: that.serviceType,
					orderType: 1,
					orderTakingUserId: that.orderTakingUserId,
					imgRemarks: that.imgRemarks.length > 0 ? that.imgRemarks.join(',') : ''
				}).then(res => {
					if (res.code == 0) {
						that.showPay = false;
						uni.showModal({
							title: '付款提示',
							content: '确认支付' + that.totalprice + '元吗?',
							complete: function(re) {
								if (re.confirm) {
									let classify = 1;
									uni.showLoading({
										title: '加载中...'
									})
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
						this.isPay = true
					}

				});
			},
			pay() {
				let that = this
				that.pays()

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
							this.isPay = true
							setTimeout(function() {
								uni.switchTab({
									url: '/pages/order/index'
								})

							}, 1000)
							uni.removeStorageSync('carlist')
						} else {
							this.isPay = true
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
						console.log(err, 12)
					}
				});
			},
		},
		// computed: {
		// 	price() {
		// 		let price = this.isVip ? this.order.memberMoney : this.order.money
		// 		console.log(price * 1 * this.value)
		// 		return (price * this.value).toFixed(2)
		// 	}
		// }
		onReachBottom: function() {
			this.page = this.page + 1;
			// this.getWaterlist()
			this.getMyList()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			// this.getWaterlist()
			this.getMyList()
		},
	}
</script>

<style>
	textarea::-webkit-input-placeholder {
		color: red;

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
		width: 200rpx;
		height: 200rpx;
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
		/* height: 200rpx; */

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

	page {
		background-color: #F5F5F5;
	}

	.types {
		width: 100%;
		height: 192rpx;
		background-color: #FFFFFF;
		border-radius: 16rpx;
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
		background-color: #FFFFFF;
	}

	.u-input {
		text-align: right !important;
	}

	.listbox {
		/* background: #FFFFFF; */
		background: #F5F5F5;
		margin: 30rpx 30rpx;
		border-radius: 24rpx;
		padding: 30rpx 0;
	}

	.btn {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 55upx;
		padding: 8upx 25upx;
	}

	.noyouhui {
		color: #FD3C44;
		border: 1rpx solid #FD3C44;
		border-radius: 15upx;
		display: inline-block;
		padding: 5rpx 20rpx;
	}
</style>