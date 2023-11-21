<template>
	<view>
		<view class="pet_list">
			<view>
				<uni-section title="宠物列表" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-grid :column="5" :showBorder="false" :highlight="false" :square="false" @change="findDoc">
					<uni-grid-item v-for="(item, index) in pet_list" :index="index" :key="index">
						<view class="grid-item-box">
							<image :src="host + item.imgUrl" mode="" class="image"></image>
							<text class="text">{{ item.name }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
		</view>
		<view class="Consultation">
			<view>
				<uni-section title="我的问诊" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list v-if="consultation_list != 0">
					<uni-list-item title="" note=""></uni-list-item>
				</uni-list>
				<view v-else style="text-align: center; background-color: white; height: 100rpx; padding: 100rpx;">
					<text>暂无查询</text>
				</view>
			</view>
		</view>
		<view class="case">
			<view>
				<uni-section title="问诊案例" sub-title="" type="line"></uni-section>
			</view>
			<view>
				<uni-list>
					<uni-list-item :title="item.doctor.name" :note="'案例描述:'+item.question" clickable v-for="(item,index) in case_list">
						<image :src="host+item.imageUrls" slot="header" style="width: 200rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
					</uni-list-item>
				</uni-list>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				pet_list: [],
				consultation_list:[],
				case_list:[]
			}
		},
		mounted() {
			this.getPetData()
			this.getConsultation()
			this.getCase()
		},
		methods: {
			getPetData() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/pet-type/list',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.pet_list = res.data.data
					},
					fail: () => {},
					complete: () => {}
				});
			},
			findDoc(item){
				uni.setStorageSync("petId",this.pet_list[item.detail.index].id)
				// console.log(this.pet_list[item.detail.index].id)
				// console.log(uni.getStorageSync("petId"))
				uni.navigateTo({
					url: '../pet_doctor/pet_doctor',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			},
			getConsultation(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/inquiry/my-list',
					method: 'GET',
					data: {},
					header:{
						Authorization:uni.getStorageSync("token")
					},
					success: res => {
						this.consultation_list = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getCase(){
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/inquiry/list?pageNum=1&pageSize=10',
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.case_list = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>
	.grid-item-box {
		flex: 1;
		// position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}

	.image {
		width: 35px;
		height: 35px;
	}
</style>
