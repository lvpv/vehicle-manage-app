<template>
    <div>
        正在登录中...
        <u-toast ref="uToast"/>
    </div>
</template>

<script>
    export default {
        data() {
            return {
            }
        },
        onLoad() {
            const that = this;
            if (that.$u.test.isEmpty(that.auth_token)) {
                dd.getAuthCode({
                    success: function (res) {
                        const code = res.authCode
                        that.$u.post('/auth/login/code', {code}).then(res => {
                            that.$u.vuex('auth_token', res);
                            that.validUser()
                        }).catch(err => { // err为服务端的返回值
                            console.log('钉钉免登失败：' + JSON.stringify(err))
                            that.$refs.uToast.show({
                                title: '登录失败，请稍后再试!',
                                type: 'error'
                            })
                        })
                    },
                    fail: function (err) {
                        console.log('获取免登授权码失败：' + JSON.stringify(err))
                        that.$refs.uToast.show({
                            title: '登录失败，请稍后再试!',
                            type: 'error'
                        })
                    }
                });
            } else {
                this.validUser()
            }
        },
        methods: {
            getUserInfo() {
                console.log('获取当前登录用户信息')
                this.$u.get('/user/info').then(res => {
                    this.$u.vuex('auth_user', res);
                    this.$refs.uToast.show({
                        title: '登陆成功',
                        type: 'success',
                        url: '/pages/home/index'
                    })
                }).catch(err => { // err为服务端的返回值
                    console.log('获取用户信息失败-->' + JSON.stringify(err))
                    this.$refs.uToast.show({
                        title: '获取用户信息失败，请稍后再试!',
                        type: 'error'
                    })
                })
            },
            validUser(){
                if (this.$u.test.isEmpty(this.auth_user)) {
                    this.getUserInfo()
                } else {
                    this.$u.route('/pages/home/index');
                }
            }
        }
    }
</script>
<style>
</style>
