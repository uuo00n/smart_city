<template>
	<view>
		<view class="">
			<view class="loop-img">
				<swiper :indicator-dots="true" :autoplay="true" :interval="3000" circular="true">
					<swiper-item>
						<image src="../../static/loop1.jpeg" mode=""></image>
					</swiper-item>
					<swiper-item>
						<image src="../../static/loop2.jpeg" mode=""></image>
					</swiper-item>
					<swiper-item>
						<image src="../../static/loop3.jpeg" mode=""></image>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<view class="find-job">
			<view>
				<uni-section title="找工作" sub-title="" type="line"></uni-section>
			</view>
			<view class="search-job">
				<uni-search-bar placeholder="请输入你要搜索的职位" @confirm="search" cancelText="搜索" @cancel="search"></uni-search-bar>
			</view>
			<view>
				<uni-section title="热门职位" sub-title="" type="line"></uni-section>
			</view>
			<view class="show-job">
				<uni-grid :column="3" :showBorder="true" :highlight="false" :square="false" @change="tap">
					<uni-grid-item v-for="(item, index) in profession.slice(0, 9)" :index="index" :key="index">
						<view class="grid-item-box">
							<text class="text">{{ item.professionName }}</text>
						</view>
					</uni-grid-item>
				</uni-grid>
			</view>
			<view class="job-list">
				<view v-if="searchCount > 0">
					<view v-if="search_obj.length != 0">
						<uni-list>
							<uni-list-item
								clickable
								@click="tapItem(item)"
								v-for="(item, index) in search_obj"
								:title="item.professionName"
								:note="
									'岗位职责：' +
									item.obligation +
									'\n' +
									'公司名称：' +
									item.companyName +
									'\n' +
									'公司地点：' +
									item.address +
									'\n' +
									' 薪资待遇：' +
									item.salary +
									'\n'
								"
							></uni-list-item>
						</uni-list>
					</view>
					<view v-else style="text-align: center; margin: 50rpx">暂无查询结果</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			job_list: [],
			search_obj: [],
			profession: [],
			searchCount: 0
		};
	},
	mounted() {
		// this.getList();
		this.getToken();
		this.getJob();
		this.getProfession();
	},
	methods: {
		// getList(){
		// 	uni.request({
		// 		url: 'http://124.93.196.45:10001/prod-api/api/job/post/list',
		// 		method: 'GET',
		// 		data: {},
		// 		success: res => {
		// 			this.job_list = res.data.rows;
		// 		},
		// 		fail: () => {},
		// 		complete: () => {}
		// 	});
		// },
		getToken() {
			uni.request({
				url: 'http://124.93.196.45:10001/prod-api/api/login',
				method: 'POST',
				data: {
					username: 'jb',
					password: 'jb'
				},
				success: (res) => {
					uni.setStorageSync('token', res.data.token);
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getJob() {
			uni.request({
				url: 'http://124.93.196.45:10001/prod-api/api/job/post/list',
				method: 'get',
				data: {},
				success: (res) => {
					this.job_list = res.data.rows;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		getProfession() {
			uni.request({
				url: 'http://124.93.196.45:10001/prod-api/api/job/profession/list',
				header: {
					Authorization: uni.getStorageSync('token')
				},
				method: 'GET',
				data: {},
				success: (res) => {
					this.profession = res.data.rows;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		search(e) {
			if (e.value === '') {
				uni.showToast({
					title: '搜索内容不能为空',
					position: 'bottom',
					icon: 'none'
				});
			} else {
				this.searchCount += 1;
				let sName = e.value;
				uni.request({
					url: 'http://124.93.196.45:10001/prod-api/api/job/post/list?name=' + sName,
					method: 'GET',
					data: {},
					success: (res) => {
						this.search_obj = res.data.rows;
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		tap(e) {
			this.searchCount += 1;
			let professionId = this.profession[e.detail.index].id;
			uni.request({
				url: 'http://124.93.196.45:10001/prod-api/api/job/post/list?professionId=' + professionId,
				method: 'GET',
				data: {},
				success: (res) => {
					this.search_obj = res.data.rows;
				},
				fail: () => {},
				complete: () => {}
			});
		},
		tapItem(e) {
			uni.setStorageSync('job_id', e.id);
			uni.navigateTo({
				url: '../job_about/job_about',
				success: (res) => {},
				fail: () => {},
				complete: () => {}
			});
		}
	},
	onNavigationBarButtonTap(e){
		uni.navigateTo({
			url: '../job_resume/job_resume',
			success: res => {},
			fail: () => {},
			complete: () => {}
		});
	},
};
</script>

<style>
.loop-img image {
	width: 100%;
}
.loop-img swiper {
	height: 200px;
}
.grid-item-box {
	flex: 1;
	// position: relative;
	/* #ifndef APP-NVUE */
	display: flex;
	/* #endif */
	flex-direction: row;
	align-items: center;
	justify-content: center;
	padding: 15px 0;
}
</style>
