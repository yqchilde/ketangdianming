<template>
    <form @submit="formSubmit">
        <cu-custom></cu-custom> 
        <view class="zai-box">
		<image src="https://ae01.alicdn.com/kf/H3959f7e56afe4fa6b62586ebc7e3e1beZ.png" mode='aspectFit' class="zai-logo"></image>
		<view class="zai-title">签到助手</view>
		<view class="zai-form">
			<input type="number" class="zai-input" name="phoneNumber" placeholder-class placeholder="请输入用户名" :value="uName"/>
			<input class="zai-input" name="password" placeholder-class password placeholder="请输入密码" :value="uPwd"/>
			<view class="zai-label" @tap="getbackPwd">忘记密码？</view>
			<button class="zai-btn" form-type="submit">立即登录</button>
			<navigator url="../register/register" hover-class="none" class="zai-label" style="padding-bottom: 0;">还没有账号？点此注册.</navigator>
		</view>
	</view>
    </form>
</template>

<script>
    var that, userid, phoneNumber, password, pageOptions;
    export default {
        data() {
            return {
                uName:'',
                uPwd:''
            }
        },
        onLoad(options) {
            that = this;
            pageOptions = options;
            // 判断打开方式是不是先通过注册打开
            if (options.mode === 'register') {
                this.uName = options.phoneNumber;
                this.uPwd = options.password;
            }
        },
        methods: {
            formSubmit: function(e) { // 提交表单进行登录
                phoneNumber = e.detail.value.phoneNumber;
                password = e.detail.value.password;
                // 正则表达式验证
                let phoneNumberReg = /^1[3456789]\d{9}$/;
                let passwordReg = /^(\w){8,16}$/;
                if (phoneNumber == '') {
                    uni.showToast({
                        title: '手机号不能为空',
                        duration: 1500,
                        icon: 'none'
                    });
                    return false;
                } else if (!phoneNumberReg.test(phoneNumber)) {
                    uni.showToast({
                        title: '手机号格式不正确',
                        duration: 1500,
                        icon: 'none'
                    });
                    return false;
                } else if (password == '') {
                    uni.showToast({
                        title: '密码不能为空',
                        duration: 1500,
                        icon: 'none'
                    });
                    return false;
                } else if (!passwordReg.test(password)) {
                    uni.showToast({
                        title: '密码长度6~20且只能由字母、数字、下划线组成',
                        duration: 1500,
                        icon: 'none'
                    });
                    return false;
                } else {
                    uni.showLoading();
                    uni.request({
                        url: that.apiServer + 'user&m=login',
                        method: 'POST',
                        header: {
                            'content-type': 'application/x-www-form-urlencoded'
                        },
                        data: {
                            phoneNumber,
                            password
                        },
                        success: res => {
                            res = res.data;
                            if (res.status == "error") {
                                uni.hideLoading();
                                uni.showToast({
                                    title:res.data,
                                    image: '/static/images/inc/icon_warning.png',
                                    duration: 2000
                                })
                                return false;
                            } else if (res.status == "ok") {
                                uni.hideLoading();
                                uni.showToast({
                                    title:'登录成功',
                                    image: '/static/images/inc/icon_success.png',
                                });
                                // 将登录信息保存到本地
                                uni.setStorageSync('USERID' , res.data.userid + '');
                                uni.setStorageSync('PHONENUMBER', res.data.phoneNumber + '');
                                uni.setStorageSync('realName', res.data.realname);
                                // 跳转
                                if (typeof(pageOptions.backtype) == 'undefined') {
                                    setTimeout(e => {
                                        uni.redirectTo({url:'../index/index'})
                                    },1500)
                                } else {
                                    setTimeout(e => {
                                        if(pageOptions.backtype == 1){
                                            uni.redirectTo({url:pageOptions.backpage});
                                        }else{
                                            uni.switchTab({url:pageOptions.backpage});
                                        }
                                    },1500);
                                }
                            }
                        },
                    });
                }
            },
            // 跳转找回密码
            getbackPwd() {
                uni.navigateTo({
                    url:'../getbackPwd/getbackPwd'
                })
            }
        }
    }
</script>

<style>
    page{
        background: white;
    }
	.zai-box{
		padding: 0 100upx;
		position: relative;
	}
	.zai-logo{
		width: 100%;
		width: 100%;
		height: 300upx;
	}
	.zai-title{
		position: absolute;
		top: 0;
		line-height: 360upx;
		font-size: 68upx;
		color: #fff;
		text-align: center;
		width: 100%;
		margin-left: -100upx;
	}
	.zai-form{
		margin-top: 250upx;
	}
	.zai-input{
		background: #e2f5fc;
		margin-top: 30upx;
		border-radius: 100upx;
		padding: 20upx 40upx;
		font-size: 36upx;
        -webkit-box-sizing: content-box;
        box-sizing: content-box;
	}
	.input-placeholder, .zai-input{
		color: #94afce;
	}
	.zai-label{
		padding: 60upx 0;
		text-align: center;
		font-size: 30upx;
		color: #a7b6d0;
	}
	.zai-btn{
		background: #ff65a3;
		color: #fff;
		border: 0;
		border-radius: 100upx;
		font-size: 36upx;
	}
	.zai-btn:after{
		border: 0;
	}
	/*按钮点击效果*/
	.zai-btn.button-hover{
		transform: translate(1upx, 1upx);
	}
</style>
