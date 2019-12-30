<template>
	<view>
		

		
	<!--	<view  :style="{background: 'url(' + imageURL+')' }"></view> -->
		
		
		
		
		<view class="tips">ta的日记:共{{count_item}}项</view>
		<view class="item" v-for="(item,index) in arr" :key="index">
			
			<checkbox :checked = "item.done" @click ="change(index)" v-if="!item.done"/>
			
			<view :class="{done: item.done}" v-if="!item.done" >{{item.value}}</view> 
			
			
			<!-- <view @click="del(index)" v-if="item.done">删除</view> -->	
		</view>
		
		
		<view>
			<view class="tips">已经完成</view>
			<button type="primary" @click="show= !show" size="mini" style="margin-right: 0;">{{show?'隐藏' : '展开'}}</button>
		</view>
		<view v-if="show">
			<view class="item" v-for="(item,index) in arr" :key="index">
				
				<checkbox :checked = "item.done" @click ="change(index)" v-if="item.done"/>
				
				<view :class="{done: item.done}" v-if="item.done" >{{item.value}}</view> 
				
				<!-- <view @click="del(index)" v-if="item.done">删除</view> -->
				
			</view>
		</view>
		
		<view class="copy_right">
			Copyright © 2019 shuitang
		</view>
		
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				//imageURL: '/static/shaobing.png',
				title: 'Hello',
				message: 'Welcome to TodoList',
				text: '',
				arr: [],
				
				show: false,
				count_item: 0,
				picture: '/static/xihu4.png'
			}
		},
		onLoad() {
			let value = uni.getStorageSync('Todolist');
			if(value ) this.arr = value;
			let temp = uni.getStorageSync('Count_item');
			if(temp) this.count_item = temp;
			//console.log(this.arr);
			//this.count_item = uni.getStorageSync('Count_item');
			//if(this.count_item<0) this.count_item = 0;
			/**
			uni.getStorage({
				key: 'Todolist',
				success: function (res){
				        console.log(res.data);
				}
			});
			*/
		},
		methods: {
			
			sure(e){
				console.log(this.arr);
				let value = e.detail.value;
				console.log(value);
					
				if(value != ''){
					this.arr.push({
						value:value,
						done:false
					});
					
					this.count_item = this.count_item+1;
					uni.setStorage({
						key: 'Todolist',
						data: this.arr
					});
					
					uni.setStorage({
						key:'Count_item',
						data: this.count_item
					});
				}
				
				
				this.text = '';
			},
			del(index){
				if(this.arr[index].done == false){
					this.count_item = this.count_item-1;
					uni.setStorage({
						key:'Count_item',
						data: this.count_item
					});
				}
				this.arr.splice(index,1);
				uni.setStorage({
					key: "Todolist",
					data: this.arr
				});
				
			},
			change(index) {
				this.arr[index].done = !this.arr[index].done;
				console.log(this.arr[index].done);
				
				if(this.arr[index].done != false){
					this.count_item = this.count_item-1;
				}
				else{
					this.count_item = this.count_item + 1;
				}
				uni.setStorage({
					key: 'Todolist',
					data: this.arr
				});
				
				uni.setStorage({
					key:'Count_item',
					data: this.count_item
				});
			    
			},
		}
	}
</script>

<style>
	page{background-color:#F1F1F2;}
	.title{
	  text-align: center;
	  font-size: 50rpx;
	  color:#555555;
	  font-weight:bold;
	  
	 }
	 .input{
	  border-bottom: 1px solid #000;
	  font-size: 20rpx;
	 }
	 .done{
	  color: #999999;
	 }
	 .item{
	  display: flex;
	  font-size: 30rpx;
	  color:#3F536E;
	  }
	 .tips{

		 font-size: 40rpx;
		 color:#000000;
		 font-weight:500;
	 }
	  
	.add_title{
		font-size: 30rpx;
	}
	.copy_right{
		font-size:15rpx;
		text-align: center;
	}
	 
</style>
