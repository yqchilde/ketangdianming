<template>
	<view>
        <view class="page-one">
            <text class="cuIcon-close" @tap="backIndex"></text>
        </view>
        <view class="header padding">
            <view class="cu-avatar xl round margin-left" style="background-image:url(/static/images/me/header-default.png);"></view>
            <text class="text-xxl padding" @tap="nameModal" data-target="editName" v-if="realName == null || realName == ''">
                点击此处设置名字
            </text>
            <text class="text-xxl padding" v-else>
                {{realName}}
            </text>
        </view>
        <view class="cu-list menu sm-border">
        	<view class="cu-item arrow" @tap="userModal" data-target="userInfo">
        		<view class="content" hover-class="none">
        			<text class="cuIcon-peoplelist text-9152fa"></text>
        			<text class="text-grey">个人信息</text>
        		</view>
        	</view>
        	<view class="cu-item arrow" @tap="classModal" data-target="setClass">
        		<view class="content" hover-class="none">
        			<text class="cuIcon-group text-9152fa"></text>
        			<text class="text-grey">绑定班级</text>
        		</view>
        	</view>
            <view class="cu-item arrow"  @tap="emailModal" data-target="setEmail">
            	<view class="content" hover-class="none">
            		<text class="cuIcon-punch text-9152fa"></text>
            		<text class="text-grey">我的邮箱</text>
            	</view>
            </view>
            <view class="cu-item arrow" @tap="pwdModal" data-target="editPwd">
            	<view class="content" hover-class="none">
            		<text class="cuIcon-unlock text-9152fa"></text>
            		<text class="text-grey">修改密码</text>
            	</view>
            </view>
            <view class="cu-item arrow" @tap="shareFid">
            	<view class="content" hover-class="none">
            		<text class="cuIcon-friendadd text-9152fa"></text>
            		<text class="text-grey">邀请好友</text>
            	</view>
            </view>
        </view>
        <!-- 修改密码 -->
        <form @submit="formEditPwd">
            <view class="cu-modal" :class="pwdModalShow=='editPwd'?'show':''">
                <view class="cu-dialog">
                    <view class="cu-bar bg-white justify-end">
                        <view class="content" style="color: #000; font-size: 35upx;">修改密码</view>
                        <view class="action" @tap="pwdHide">
                            <text class="cuIcon-close text-red"></text>
                        </view>
                    </view>
                    <view>
                        <view class="padding-sm">
                            <input password name="newpwd" style="font-size: 30upx;" placeholder="请输入你的新密码" />
                        </view>
                        <view class="bg-white" style="height: 25rpx;"></view>
                        <view class="padding-sm">
                            <input password name="renewpwd" style="font-size: 30upx;" placeholder="请再次输入你的新密码" />
                        </view>
                        
                    </view>
                    <view class="cu-bar bg-white justify-end">
                        <view class="action">
                            <button class="cu-btn line-green text-green" @tap="pwdHide">取消</button>
                            <button form-type="submit" class="cu-btn bg-green margin-left" @tap="pwdHide">确定</button>
                        </view>
                    </view>
                </view>
            </view>
        </form>
        
        <!-- 编辑真实姓名 -->
        <form @submit="formEditName">
            <view class="cu-modal" :class="nameModalShow=='editName'?'show':''">
                <view class="cu-dialog">
                    <view class="cu-bar bg-white justify-end">
                        <view class="content" style="color: #000; font-size: 35upx;">修改名字</view>
                        <view class="action" @tap="nameHide">
                            <text class="cuIcon-close text-red"></text>
                        </view>
                    </view>
                    <view class="padding">
                        <input type="text" name="realName" style="font-size: 30upx;" placeholder="请输入真实姓名,只有一次机会" />
                    </view>
                    <view class="cu-bar bg-white justify-end">
                        <view class="action">
                            <button class="cu-btn line-green text-green" @tap="nameHide">取消</button>
                            <button form-type="submit" class="cu-btn bg-green margin-left" @tap="nameHide">确定</button>
                        </view>
                    </view>
                </view>
            </view>
        </form>
        
        <!-- 绑定班级 -->
        <form @submit="formSetClass">
            <view class="cu-modal" :class="classModalShow=='setClass'?'show':''">
                <view class="cu-dialog">
                    <view class="cu-bar bg-white justify-end">
                        <view class="content" style="color: #000; font-size: 35upx;">绑定班级</view>
                        <view class="action" @tap="classHide">
                            <text class="cuIcon-close text-red"></text>
                        </view>
                    </view>
                    <view class="padding">
                        <view v-if="className == null || className == ''">
                            <input type="text" name="class" style="font-size: 30upx;" placeholder="绑定你的班级" />
                        </view>
                        <view v-else>
                            <input type="text" name="class" style="font-size: 30upx;" placeholder="绑定你的班级" :value="className"/>
                        </view>
                    </view>
                    <view class="cu-bar bg-white justify-end">
                        <view class="action">
                            <button class="cu-btn line-green text-green" @tap="classHide">取消</button>
                            <button form-type="submit" class="cu-btn bg-green margin-left" @tap="classHide">确定</button>
                        </view>
                    </view>
                </view>
            </view>
        </form>
        
        <!-- 绑定邮箱 -->
        <form @submit="formSetEmail">
            <view class="cu-modal" :class="emailModalShow=='setEmail'?'show':''">
                <view class="cu-dialog">
                    <view class="cu-bar bg-white justify-end">
                        <view class="content" style="color: #000; font-size: 35upx;">绑定邮箱</view>
                        <view class="action" @tap="emailHide">
                            <text class="cuIcon-close text-red"></text>
                        </view>
                    </view>
                    <view class="padding">
                        <view v-if="email == null || email == ''">
                            <input type="text" name="email" style="font-size: 30upx;" placeholder="绑定你的邮箱" />
                        </view>
                        <view v-else>
                            <input type="text" name="email" style="font-size: 30upx;" placeholder="绑定你的邮箱" :value="email"/>
                        </view>
                    </view>
                    <view class="cu-bar bg-white justify-end">
                        <view class="action">
                            <button class="cu-btn line-green text-green" @tap="emailHide">取消</button>
                            <button form-type="submit" class="cu-btn bg-green margin-left" @tap="emailHide">确定</button>
                        </view>
                    </view>
                </view>
            </view>
        </form>
        
        <!-- 个人信息 -->
        <view class="cu-modal" :class="userModalShow=='userInfo'?'show':''">
        	<view class="cu-dialog">
        		<view class="cu-bar bg-white justify-end">
        			<view class="content">个人信息</view>
        			<view class="action" @tap="userHide">
        				<text class="cuIcon-close text-red"></text>
        			</view>
        		</view>
        		<view class="padding-xl text-left">
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        UID:{{userId}}
                    </view>
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        手机号:{{phoneNumber}}
                    </view>
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        姓名:{{realName}}
                    </view>
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        邮箱:{{email}}
                    </view>
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        班级:{{className}}
                    </view>
                    <view style="font-size: 30upx;margin-bottom: 10upx;">
                        注册时间:{{regTime}}
                    </view>
        		</view>
        	</view>
        </view>
	</view>
</template>

<script>
    var that;
	export default {
		data() {
			return {
                userId: '', //UID
                phoneNumber: '', //手机号
                realName: '', //姓名
                email: '', //邮箱
                className: '', //班级
                regTime: '', //注册时间
                nameModalShow: null,
                userModalShow: null,
                classModalShow: null,
                emailModalShow: null,
                pwdModalShow: null
			}
		},
        onLoad() {
            that = this;
            let loginRes = this.checkLogin('../me/me', 1);
            if(!loginRes){return ;}
            let userid = uni.getStorageSync('USERID');
            let phoneNumber = uni.getStorageSync('PHONENUMBER');
            uni.request({
                url: that.apiServer + 'user&m=getUserInfo',
                method: 'POST',
                header: {
                    'content-type': 'application/x-www-form-urlencoded'
                },
                data: {
                    userid,
                    phoneNumber
                },
                success: res => {
                    res = res.data;
                    if (res.status == 'ok') {
                        this.userId = res.data.userid;
                        this.phoneNumber = res.data.phoneNumber;
                        this.realName = res.data.realname;
                        this.email = res.data.email;
                        this.className = res.data.class;
                        let date = new Date(res.data.regtime * 1000);
                        this.regTime = date.getFullYear()+'-'+(date.getMonth()+1)+'-'+ date.getDate()+' '+date.getHours()+':'+ date.getMinutes()+':'+date.getSeconds();
                    } else if (res.status == 'error') {
                        uni.showToast({title:res.data})
                    }
                },
            });  
        },
		methods: {
			nameModal: function(e) { //姓名modal
                this.nameModalShow = e.currentTarget.dataset.target
            },
            nameHide: function (e) {
            	this.nameModalShow = null
            },
            userModal: function(e) { //信息modal
                this.userModalShow = e.currentTarget.dataset.target
            },
            userHide: function (e) {
            	this.userModalShow = null
            },
            classModal: function(e) { //班级modal
                this.classModalShow = e.currentTarget.dataset.target
            },            
            classHide: function (e) {
            	this.classModalShow = null
            },
            emailModal: function(e) { //邮箱modal
                this.emailModalShow = e.currentTarget.dataset.target
            },            
            emailHide: function (e) {
            	this.emailModalShow = null
            },
            pwdModal: function(e) { //密码modal
                this.pwdModalShow = e.currentTarget.dataset.target
            },            
            pwdHide: function (e) {
            	this.pwdModalShow = null
            },
            formEditName: function(e) {
                let userId = uni.getStorageSync('USERID');
                let realName = e.detail.value.realName;
                let realNameReg = /^[\u4e00-\u9fa5]{2,4}$/;
                if (realName == '') {
                    uni.showToast({title: '姓名不能为空',icon: 'none'});
                    return false;
                } else if (!realNameReg.test(realName)) {
                    uni.showToast({title: '请输入汉字,且长度为2-4位',icon: 'none'});
                    return false;
                }
                uni.request({
                    url: that.apiServer + 'user&m=editName',
                    method: 'POST',
                    header: {
                        'content-type': 'application/x-www-form-urlencoded'
                    },
                    data: {
                        userId,
                        realName
                    },
                    success: res => {
                        res = res.data;
                        if (res.status == 'ok') {
                            uni.showToast({title:res.data,icon:'none'});
                            this.realName = realName;
                            if (uni.getStorageSync('realName')) {
                                uni.removeStorageSync('realName');
                            }
                            uni.setStorageSync('realName',realName);
                        } else if (res.status == 'error') {
                            uni.showToast({title:res.data,icon:'none'})
                        }
                    },
                });
            },
            formSetClass: function(e) {
                let userId = uni.getStorageSync('USERID');
                let className = e.detail.value.class;
                if (className == '') {
                    uni.showToast({title: '班级不能为空',icon: 'none'});
                    return false;
                }
                uni.request({
                    url: that.apiServer + 'user&m=setClass',
                    method: 'POST',
                    header: {
                        'content-type': 'application/x-www-form-urlencoded'
                    },
                    data: {
                        userId,
                        className
                    },
                    success: res => {
                        res = res.data;
                        if (res.status == 'ok') {
                            uni.showToast({title:res.data,icon:'none'});
                            this.className = className;
                        } else if (res.status == 'error') {
                            uni.showToast({title:res.data,icon:'none'})
                        }
                    },
                });
            },
            formSetEmail: function(e) {
                let userId = uni.getStorageSync('USERID');
                let email = e.detail.value.email;
                let emailReg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
                if (email == '') {
                    uni.showToast({title: '邮箱不能为空',icon: 'none'});
                    return false;
                } else if (!emailReg.test(email)) {
                    uni.showToast({title: '邮箱格式不正确',icon: 'none'});
                    return false;
                }
                uni.request({
                    url: that.apiServer + 'user&m=setEmail',
                    method: 'POST',
                    header: {
                        'content-type': 'application/x-www-form-urlencoded'
                    },
                    data: {
                        userId,
                        email
                    },
                    success: res => {
                        res = res.data;
                        if (res.status == 'ok') {
                            uni.showToast({title:res.data,icon:'none'});
                            this.email = email;
                        } else if (res.status == 'error') {
                            uni.showToast({title:res.data,icon:'none'})
                        }
                    },
                });
            },
            formEditPwd(e) {
                let userId = uni.getStorageSync('USERID');
                let phoneNumber = uni.getStorageSync('PHONENUMBER');
                let newpwd = e.detail.value.newpwd;
                let renewpwd = e.detail.value.renewpwd;
                let passwordReg = /^(\w){8,16}$/;
                
                if (newpwd == '') {
                    uni.showToast({title:'密码不能为空',icon:'none',position:'bottom'});
                    return false;
                } else if (!passwordReg.test(newpwd)) {
                    uni.showToast({title:'密码为8到16位',icon:'none',position:'bottom'});
                    return false;
                } else if (renewpwd != newpwd) {
                    uni.showToast({title:'两次密码不一致',icon:'none',position:'bottom'});
                    return false;
                }
                uni.request({
                    url: that.apiServer + 'user&m=editPwd',
                    method: 'POST',
                    header: {
                        'content-type': 'application/x-www-form-urlencoded'
                    },
                    data: {
                        userId,
                        phoneNumber,
                        newpwd
                    },
                    success: res => {
                        res = res.data;
                        if (res.status == 'ok') {
                            uni.showToast({title:res.data,icon:'none'});
                            setTimeout((e) => {
                                uni.reLaunch({
                                    url:'../login/login'
                                })
                            },1000);
                        } else if (res.status == 'error') {
                            uni.showToast({title:res.data,icon:'none'})
                        }
                    },
                });
            },
            // 右上角叉号返回
            backIndex() {
                uni.navigateBack({
                    delta:1
                })
            },
            // 分享好友下载地址
            shareFid() {
                uni.request({
                    url: that.apiServer + 'user&m=shareDown',
                    method: 'GET',
                    data: {},
                    success: res => {
                        res = res.data;
                        if (res.status == 'ok') {                            
                            uni.setClipboardData({
                                data: res.data,
                                success: function () {
                                    uni.showToast({title:'已成功复制下载地址',icon:'none',position:'bottom'});
                                }
                            });
                        }
                    },
                });
            }
		},
	}
</script>

<style>
    page{
        background: white;
    }
    .page-one {
        height: 130rpx;
    }
    .page-one .cuIcon-close {
        position: fixed;
        right: 50rpx;
        top: 50rpx;
        font-size: 60rpx;
    }
</style>
