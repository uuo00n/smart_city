<template>
	<view>
		<view class="search-box">
			<uni-search-bar @confirm="search" cancelText="确认" @cancel="search"></uni-search-bar>
		</view>
		<view class="bd">
			<view v-if="this.list.length === 0" style="text-align: center; margin: 100rpx;">
				<text>暂无查询结果</text>
			</view>
			<view v-else>
				<uni-list>
					<uni-list-item clickable :title="'车牌号：'+item.plateNumber" 
					:note="'入场时间：'+item.entryTime+'\n'+'出场时间：'+item.outTime+'\n'+'停车场名称：'+item.parkName+'\n'+'消费金额：'+item.monetary+'元'" 
					v-for="(item,index) in shortData"></uni-list-item>
					<button type="default" style="margin: 10rpx; background-color: white;" @click="showMoreData()">查看更多</button>
				</uni-list>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[],
				displayCount:5,
				visibleCount:5
			}
		},
		mounted() {
			this.getData()
		},
		methods: {
			search(res) {
				let text = res.value
				let carNum = '辽A12345'
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/park/lot/record/list?entryTime='+text+'&plateNumber='+carNum,
					method: 'GET',
					data: {},
					success: res => {
						if(this.list.length === 0){
							uni.request({
								url: 'http://124.93.196.45:10001/prod-api/api/park/lot/record/list?outTime='+text+'&plateNumber='+carNum,
								method: 'GET',
								data: {},
								success: res => {
									this.list = res.data.rows
									console.log("1")
								},
								fail: () => {},
								complete: () => {}
							});
						}else{
							this.list = res.data.rows
							console.log("2")
						}
					},
					fail: () => {},
					complete: () => {}
				});
				
			},
			getData(){
				let carNum = '辽A12345'
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/park/lot/record/list?plateNumber='+carNum,
					method: 'GET',
					data: {},
					success: res => {
						this.list = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
			showMoreData(){
				this.visibleCount += 5
			}
		},
		computed:{
			shortData(){
				return this.list.slice(0,this.visibleCount)
			}
		}
	}
</script>

<style>

</style>
