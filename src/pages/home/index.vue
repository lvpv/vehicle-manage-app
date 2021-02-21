<template>
    <div>
        这里是主页
        {{ auth_user.realName }}
    </div>
</template>

<script>
    export default {
        data() {
            return {
                user: ''
            }
        },
        onLoad() {
            this.user = JSON.stringify(this.auth_user)
        },
        methods: {

        }
    }
</script>

<style>
</style>
