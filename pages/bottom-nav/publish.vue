<template>
	<view class="uni-page-body">
		<view class="feedback-body">
			<input class="feedback-input" v-model="sendDate.contact" placeholder="日记的标题是…" />
		</view>
		<view class='feedback-title feedback-star-view'>
			<text>今日心情</text>
			<view class="feedback-star-view">
				<text class="feedback-star" v-for="(value,key) in stars" :key="key" :class="key < sendDate.score ? 'active' : ''"
				 @tap="chooseStar(value)"></text>
			</view>
		</view>
		<view class="feedback-body">
			<textarea placeholder="请输入…" v-model="sendDate.content" class="feedback-textare" />
			</view>

        <choose :count="count"  :imgList="imgList"  @changes="fileChange"></choose>
        <compress  ref="compress" :maxwh="maxwh" :quality="quality" > </compress>


        <view class="swiper-list">
            <view class="uni-list-cell uni-list-cell-pd feedback-title">
                <view class="uni-list-cell-db ">压缩图片</view>
                <switch :checked="isYasuo" @change="changeIndicatorDots" />
            </view>
        </view>
        <button type="default" class="feedback-submit" @tap="send">提交</button>

    </view>
</template>

<script>
    import choose from "../../components/template/image/choose.vue"
    import compress from "../../components/template/image/compress.vue"
    export default {
        name:'newsPublish',
        components:{
        	choose,
            compress
        },
        data() {
            return {
                isYasuo:true,
                count:9,
                maxwh:280,
                quality:1, 
                
                msgContents: ["界面显示错乱", "启动缓慢，卡出翔了", "UI无法直视，丑哭了", "偶发性崩溃"],
                stars: [1, 2, 3, 4, 5],
                imgList: [],
                sendDate: {
                    score: 0,
                    content: "",
                    contact: ""
                }
            }
        },
        onLoad() {

        },
        methods: {
            compressImg(e){
              console.log(e)  
            },
            changeIndicatorDots(e){
            this.isYasuo = !this.isYasuo
            },
            fileChange(e){
              this.imgList=e;
                  //             YCImg.canvasToBase64(e)
                  // .then(e => {
                  //     // console.log(JSON.stringify(e))
                  //     this.value = e;
                  //     this.confirm();
                  //     // console.log(e)
                  // })
                  // .catch(e => {
                  //     uni.showToast({
                  //         title: '失败！' + e.message,
                  //         icon: 'none',
                  //         duration: 1000
                  //     });
                  // })
            },
            chooseStar(e) { //点击评星
                this.sendDate.score = e;
            },
            previewImage() { //预览图片
                uni.previewImage({
                    urls: this.imgList
                });
            },
            send() { //发送提交
                // console.log(JSON.stringify(this.sendDate));
                
                function requst(imgs,data){
                    console.log(JSON.stringify(imgs));
                                    uni.uploadFile({
                        url: "https://service.dcloud.net.cn/feedback",
                        files: imgs,
                        formData: data,
                        success: (res) => {
                            if (res.statusCode === 200) {
                                uni.showToast({
                                    title: "反馈成功!"
                                });
                                this.imgList = [];
                                this.sendDate = {
                                    score: 0,
                                    content: "",
                                    contact: ""
                                }
                            }
                        },
                        fail: (res) => {
                            // console.log(res)
                        }
                    });
                }
                
                if(this.isYasuo){

                    this.$refs.compress.yasuoImg(this.imgList).then(e=>{
                        // console.log([this.imgList,e])
                        let imgs = e.map((value, index) => {
                            //var base64= value.tempFilePath
                            return {
                                name: "image" + index,
                                uri:value.path,
                                base64:value.tempFilePath
                                
                            }
                        })
                        // console.log(imgs)
                        requst(imgs,this.sendDate)
                    })
                }else{
                    
                    let imgs = this.imgList.map((value, index) => {
                        return {
                            name: "image" + index,
                            uri:value.path,
                            base64:value.tempFilePath
                        }
                    })
                    requst(imgs,this.sendDate)
                }
            }
        }
    }
</script>

<style>
	@import "../../common/uni.css";
</style>
