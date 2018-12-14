<template>
	<view class="index-page"> 
		<button type="default" :size="defaultSize" :loading="loading" :disabled="disabled" @tap="sayHello" hover-class="other-button-hover"> default </button>
		<button type="warning" :size="defaultSize" :loading="loading" :disabled="disabled" @tap="uploadFile" hover-class="other-button-hover"> 上传 </button>

		<navigator url="../insidePage/insidePage" hover-class="navigator-hover">
				<button type="default">跳转到新页面 {{text}} {{num}}</button>
		</navigator>
		<view class="list-container">
		<view v-for="(item, index) in list" class="list-item" :key="index">
			<view class="img-wrap">
				<image class="list-item-img" v-if="item && item.posters && item.posters.thumbnail" :src="item.posters.thumbnail"></image>
			</view>
			<text class="list-item-text">{{ item.title }}</text>
		</view></view>
	</view>
</template>

<script>
var types = ['default', 'primary', 'warn'];
var pageObject = {
    data() {
        return {
            defaultSize: 'default',
            primarySize: 'default',
            warnSize: 'default',
            disabled: false,
            plain: false,
            loading: false,
            text: '',
						num: 0,
            list: []
        };
    },
    mounted() {
        this.getData();
    },
		onLoad: function (options) {
        setTimeout(function () {
            console.log('start pulldown');
        }, 1000);
        uni.startPullDownRefresh();
    },
		onPullDownRefresh (){
			setTimeout(() => {
				this.text = 'fuck'
				this.num+=1
						uni.stopPullDownRefresh();
				
			}, 2000)
		},
    methods: {
        getData() {
            uni.request({
                url:
                    'https://raw.githubusercontent.com/facebook/react-native/0.51-stable/docs/MoviesExample.json', //仅为示例，并非真实接口地址。
                success: res => {
                    console.log(res.data);
                    let data = res.data || {};
                    this.text = JSON.stringify(data);
                    this.list = data.movies || [];
                }
            });
        },
				
				uploadFile(){
					uni.chooseImage({
    success: (chooseImageRes) => {
        const tempFilePaths = chooseImageRes.tempFilePaths;
        uni.uploadFile({
            url: 'https://www.example.com/upload', //仅为示例，非真实的接口地址
            filePath: tempFilePaths[0],
            name: 'file',
            formData: {
                'user': 'test'
            },
            success: (uploadFileRes) => {
                console.log(uploadFileRes.data);
            }
        });
    }
});
				},
        sayHello() {
            uni.showModal({
                title: '提示',
                content: '这是一个模态弹窗',
                success: function(res) {
                    if (res.confirm) {
                        console.log('用户点击确定');
                    } else if (res.cancel) {
                        console.log('用户点击取消');
                    }
                }
            });
        },
        setDisabled: function(e) {
            this.disabled = !this.disabled;
        },
        setPlain: function(e) {
            this.plain = !this.plain;
        },
        setLoading: function(e) {
            this.loading = !this.loading;
        }
    }
};

export default pageObject;
</script>

<style lang="less">
.logo {
    height: 200upx;
    width: 200upx;
    margin-top: 200upx;
}

.title {
    font-size: 36upx;
    color: #8f8f94;
}

.list-container {
	padding: 15px;
}
.list-item {
    display: flex;
    // align-items: center;
    padding: 8px 15px;
	border: 1px solid #eaeaea;
	margin-bottom: 8px;

    .img-wrap {
        width: 80px;
        height: 80px;
        margin-right: 15px;
        .list-item-img {
       width: 80px;
       height: 80px;
        }
    }
    .list-item-text {
	flex: 1;
		font-size: 14px;
		taxt-align: left;
    }
}
</style>
