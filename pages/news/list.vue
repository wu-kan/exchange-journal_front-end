<template>
	<view class="container">
		
		
		<view class="search">
			<!--<image src="../../static/zy-search/voice.svg" mode="aspectFit" @click="startRecognize()" class="voice-icon"></image> -->
			<input maxlength="20" focus type="text" value="" confirm-type="search" @confirm="get_diary" placeholder="输入关键字" v-model.trim="searchText"/>
			<image src="../../static/zy-search/search.svg" mode="aspectFit" @click="getname_link()" class="search-icon"></image>
		</view>
		{{straghtline}}
		<view class="list-view">
			<view v-for="(item,index) in newsList" :key="index" class="list-cell list-item" :class="[(newsList.length-1)==index?'last':'']"
			 hover-class="hover" :hover-stay-time="150" @tap="detail">
				<view class="cell-title-box" :class="[item.img==0?'':'min']">
					<view class="cell-title" :class="[item.img==0?'pdr0':'']">{{item.title}}</view>
					<image :src="'../../static/images/product/'+item.img+'.jpg'" class="img" v-if="item.img!=0"></image>
				</view>
				<view class="sub-title">
					<text class="tag" :class="[getLabelCss(item.label)]" v-if="item.label!=0">{{getLabelText(item.label)}}</text>
					<text class="sub-content">{{item.source}}</text>
				</view>
			</view>

		</view>
		<!--加载loadding-->
		<tui-loadmore :visible="loadding"></tui-loadmore>
		<tui-nomore :visible="!pullUpOn"></tui-nomore>
		<!--加载loadding-->
	</view>
</template>

<script>
	import tuiLoadmore from "@/components/loadmore/loadmore"
	import tuiNomore from "@/components/nomore/nomore"
	export default {
		components: {
			tuiLoadmore,
			tuiNomore
		},
		data() {
			return {
				straghtline: '--------------------------------------------------------',
				pageIndex: 1,
				newsList: [{
					title: '今日开心',
					img: 0,
					source: "今天受到了同学的赞赏，真的超级开心，不知道你过度怎么样？",
					label: 0
				}, {
					title: '好多DDL',
					img: 0,
					source: "为什么有那么多的作业要去写呀，真的做不完了，\n 不知道你怎么样？",
					label: 0
				},{
					title: '多向往多漫长',
					img: 0,
					source: "我多想能多陪你一场，把前半生的事迹跟你讲",
					label: 0
					
				},
				{
					title: '今日开心',
					img: 0,
					source: "今天受到了同学的赞赏，真的超级开心，不知道你过度怎么样？",
					label: 0
				}, {
					title: '好多DDL',
					img: 0,
					source: "为什么有那么多的作业要去写呀，真的做不完了，\n 不知道你怎么样？",
					label: 0
				},{
					title: '多向往多漫长',
					img: 0,
					source: "我多想能多陪你一场，把前半生的事迹跟你讲",
					label: 0
					
				},
				{
					title: '今日开心',
					img: 0,
					source: "今天受到了同学的赞赏，真的超级开心，不知道你过度怎么样？",
					label: 0
				}, {
					title: '好多DDL',
					img: 0,
					source: "为什么有那么多的作业要去写呀，真的做不完了，\n 不知道你怎么样？",
					label: 0
				},{
					title: '多向往多漫长',
					img: 0,
					source: "我多想能多陪你一场，把前半生的事迹跟你讲",
					label: 0
					
				},
				{
					title: '今日开心',
					img: 0,
					source: "今天受到了同学的赞赏，真的超级开心，不知道你过度怎么样？",
					label: 0
				}, {
					title: '好多DDL',
					img: 0,
					source: "为什么有那么多的作业要去写呀，真的做不完了，\n 不知道你怎么样？",
					label: 0
				},{
					title: '多向往多漫长',
					img: 0,
					source: "我多想能多陪你一场，把前半生的事迹跟你讲",
					label: 0
					
				},
				],
				loadData: [{
					title: '今日开心',
					img: 0,
					source: "今天受到了同学的赞赏，真的超级开心，不知道你过度怎么样？",
					label: 0
				}, {
					title: '好多DDL',
					img: 0,
					source: "为什么有那么多的作业要去写呀，真的做不完了，\n 不知道你怎么样？",
					label: 0
				},
				],
				loadding: false,
				pullUpOn: true
			}
		},
		methods: {
			detail(e) {
				uni.navigateTo({
					url: '../extend-view/newsDetail/newsDetail'
				})
			},
			getLabelText: function(label) {
				return ["", "要闻", "朋友都看过", "本地资讯", "互联网精英看过"][label];
			},
			getLabelCss: function(label) {
				return ["", "b-red", "b-blue", "b-orange", "b-green"][label];
			},
			
			get_diary: function(label){
				return [" "];
			},
			
			
			
			
		},
		//页面相关事件处理函数--监听用户下拉动作
		onPullDownRefresh: function() {
			//延时为了看效果
			setTimeout(() => {
				this.newsList = this.loadData;
				this.pageIndex = 1;
				this.pullUpOn = true;
				this.loadding = false;
				uni.stopPullDownRefresh();
				this.tui.toast("刷新成功");
			}, 200)
		},

		// 页面上拉触底事件的处理函数
		onReachBottom: function() {
			if (!this.pullUpOn) return;
			this.loadding = true;
			if (this.pageIndex == 3) {
				this.loadding = false;
				this.pullUpOn = false;
			} else {
				this.newsList = this.newsList.concat(this.loadData);
				this.pageIndex = this.pageIndex + 1;
			}
		}
	}
</script>

<style>
	
	
	.container {
		padding-bottom: env(safe-area-inset-bottom);
	}
	.search-icon{
		width: 36upx;
		height: 36upx;
		padding: 16upx 20upx 16upx 0;
		position: absolute;
		right: 0;
		top: -2upx;
		z-index: 10;
	}
	.voice-icon{
		width: 36upx;
		height: 36upx;
		padding: 16upx 20upx 16upx 0;
		position: absolute;
		left: 16upx;
		top: -2upx;
		z-index: 10;
	}
	
	.search{
		width: 640upx;
		margin: 30upx auto 0;
		position: relative;
		input{
			background-color: #F7F7F7;
			padding: 10upx 74upx;
			font-size: 28upx;
			border-radius: 50upx;
		}
		
	}
	
	
	
	.list-view {
		width: 100%;
		background: #fff;
		box-sizing: border-box;
	}

	.list-cell {
		padding: 30upx 32upx;
		box-sizing: border-box;
	}

	.cell-title-box {
		position: relative;
	}

	.min {
		min-height: 90upx
	}

	.cell-title {
		padding-right: 172upx;
		font-size: 36upx;
		line-height: 56upx;
		word-break: break-all;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 3;
	}

	.pdr0 {
		padding-right: 0 !important;
	}

	.img {
		position: absolute;
		right: 0;
		top: 6upx;
		width: 146upx;
		height: 146upx;
		border-radius: 4upx;
	}

	.sub-title {
		padding-top: 24upx;
		font-size: 28upx;
		color: #BCBCBC;
		display: flex;
		align-items: center
	}

	.tag {
		padding: 5upx 10upx;
		font-size: 24upx;
		border-radius: 4upx;
		margin-right: 20upx;
	}

	.b-red {
		background: #FCEBEF;
		color: #8A5966;
	}

	.b-blue {
		background: #ECF6FD;
		color: #4DABEB;
	}

	.b-orange {
		background: #FEF5EB;
		color: #FAA851
	}

	.b-green {
		background: #E8F6E8;
		color: #44CF85
	}
</style>
