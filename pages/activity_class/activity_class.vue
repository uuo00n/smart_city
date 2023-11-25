<template>
	<view>
		<view class="sec">
			<uni-section :title="data.name+'服务'" sub-title="" type="line">	</uni-section>
		</view>
		<view class="body">
			<uni-list>
				<uni-list-item :title="item.name" :note="'报名人数:'+item.signupNum+'人'+' '+'点赞数'+item.likeNum+'人'"
					clickable v-for="(item,index) in atcList" @click="">
					<image :src="host+item.imgUrl" slot="header"
						style="width: 230rpx; height: 150rpx; border-radius: 5px; margin-right: 10px;"></image>
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
				data:[],
				atcList: []
			}
		},
		mounted() {
			this.getClassData()
			this.getActList()
		},
		methods: {
			getClassData(){
				this.data = uni.getStorageSync("actClass")
			},
			getActList() {
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/activity/activity/list?categoryId='+this.data.id,
					method: 'GET',
					data: {},
					success: res => {
						this.atcList = res.data.rows
					},
					fail: () => {},
					complete: () => {}
				});
			},
		}
	}
</script>

<style>

</style>
