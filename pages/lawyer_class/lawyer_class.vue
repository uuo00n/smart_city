<template>
	<view>
		<view>
			<uni-section :title="classData.name+'专长'" sub-title="" type="line"></uni-section>
		</view>
		<view>
			<uni-list>
				<uni-list-item v-for="(item,index) in listData" :key="index">
					<view slot="body" style="margin-right: 20%;">
						<view>
							<view>{{item.name}}</view>
							<view style="font-size: 25rpx;">
								<view>从业年限：{{item.workStartAt}}</view>
								<view>咨询人数：{{item.serviceTimes}}人</view>
								<view>法律专长：{{item.legalExpertiseName}}</view>
								<view>好评率：{{item.favorableRate}}%</view>
							</view>
						</view>	
					</view>
					<image :src="host+item.avatarUrl" slot="header"
						style="width: 150rpx; height: 200rpx; border-radius: 5px; margin-right: 10px;"></image>
					<view slot="footer"  style="display: flex;justify-items: flex-end; align-items: flex-end;">
						<button type="primary" size="mini" @click="goAbout(item)">咨询</button>
					</view>
				</uni-list-item>
			</uni-list>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				host: 'http://124.93.196.45:10001',
				classData:[],
				listData:[]
			}
		},
		mounted() {
			this.getList()
		},
		methods: {
			getList(){
				this.classData = uni.getStorageSync("law_class")
				console.log(this.classData)
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/lawyer-consultation/lawyer/list?legalExpertiseId='+this.classData.id,
					method: 'GET',
					data: {},
					header: {
						Authorization: uni.getStorageSync("token")
					},
					success: res => {
						this.listData = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goAbout(e){
				uni.setStorageSync("lawyer_msg",e)
				uni.navigateTo({
					url: '../lawyer_about/lawyer_about',
					success: res => {},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
