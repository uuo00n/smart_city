<template>
	<view>
		<!-- 将列表与折叠面板融合的写法 -->
		<uni-collapse accordion v-model="accordionVal">
			<view v-for="(item,index) in doc" >
				<uni-list-item :title="item.name" 
					:note="'职称：'+item.jobName+'\n'+
					'执业编号：'+item.practiceNo+'\n'+
					'从业年限：'+item.workingYears+'年'"
					clickable @click="goInquiry(item)">
					<image :src="host+item.avatar" slot="header" style="width: 200rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
				</uni-list-item>
				<uni-collapse-item title="医生擅长" style="background-color: white;" :show-animation="true" >
					<view style="padding: 30rpx;" >
						<text>{{item.goodAt}}</text>
					</view>
				</uni-collapse-item>
			</view>
		</uni-collapse>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: "http://124.93.196.45:10001",
				doc: [],
				accordionVal:'1'
			}
		},
		mounted() {
			this.getDocList()
		},
		methods: {
			getDocList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/pet-hospital/pet-doctor/list?typeId=' + uni
						.getStorageSync("petId"),
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync('token')
					},
					success: res => {
						this.doc = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goInquiry(item){
				uni.setStorageSync("doc_msg",item)
				uni.navigateTo({
					url: '../pet_inquiry/pet_inquiry',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
				// console.log(uni.getStorageSync("doc_msg"))
			}
		}
	}
</script>

<style>

</style>
