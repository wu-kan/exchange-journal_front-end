<template>
	<view class="content">
		<view>
			<text>
				下面应该放一张合适的联系人的那种图片。
			</text>
		</view>
		<image class="logo" src="/static/nk.jpg"></image>
		<view class="loggin_area">
			
			<input class="user_name"  value="" placeholder="账号" @input="getUsrName"/>
			<input class="password" value="" password="true" placeholder="密码" @input="getPassword" confirm-type="done"/>
			<button @click="logging()">登录</button>
 			
			<button @click="newUser()">新用户</button>
			<button @click="changePass()">忘记密码</button>
				
				
			
		
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				user_name:'', //保存用户名
				password: '' //保存密码
			}
		},
		onLoad() {

		},
		methods: {
			getUsrName:function(e){
				let v = e.detail.value;
				console.log(v);
				this.user_name = v;
			},
			getPassword:function(e){
				//密码是否考虑加密？？？
				let v = e.detail.value;
				console.log(v);
				this.password = v;
			},
			logging:function(){
				console.log(this.user_name);
				console.log(this.password);
				//验证用户名和密码是否一致
				//根据用户名，查询数据库，和密码对比
				
				
				
				//ok，则跳转下一个界面
				
				
				//否则，弹出错误信息，停留在原界面。
				//alert('错误！');
			},
			newUser:function(){
				console.log("newUser page!");
			},
			changePass:function(){
				console.log("change the pass");
			},
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
