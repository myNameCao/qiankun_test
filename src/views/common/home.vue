<template>
  <div class="home">
    <div>测试头部</div>
    <!-- <div id="vueApp" class="content"></div> -->
    <div id="vueapp2" class="content"></div>
  </div>
</template>

<script>
  import axios, { setCookie } from '@utils'

  import { registerMicroApps, start } from 'qiankun'

  registerMicroApps([
    {
      name: 'vueapp2',
      entry: '//localhost:8081',
      container: '#vueapp2',
      activeRule: '/question'
    }
    // {
    //   name: 'vueApp',
    //   entry: '//localhost:8080',
    //   container: '#vueApp',
    //   activeRule: '/admin'
    // }
    // {
    //   name: 'angularApp',
    //   entry: '//localhost:4200',
    //   container: '#container',
    //   activeRule: '/app-angular'
    // }
  ])
  // 启动 qiankun
  export default {
    data() {
      return {
        inputText: '',
        readonly: true,
        inputType: 'text',
        params: {
          userCode: '',
          password: '',
          isRememberPwd: false
        },
        errorTip: '',
        userInfo: [],
        loginStatus: false,
        loading: false,
        redirect: undefined,
        yzUserName: ''
      }
    },
    watch: {
      $route: {
        handler: function(route) {
          this.redirect = route.query && route.query.redirect
        },
        immediate: true
      }
    },
    created() {
      setTimeout(function() {
        document.body.classList.add('on-start')
      }, 100)
      setTimeout(function() {
        document.body.classList.add('document-loaded')
      }, 1800)
    },
    mounted() {
      if (!window.qiankunStarted) {
        window.qiankunStarted = true
        start()
      }
    },
    methods: {
      loginHandler() {
        this.errorTip = ''
        if (this.params.userCode == '') {
          this.errorTip = '请输入登录账户!'
          return
        }
        if (this.params.password == '') {
          this.errorTip = '请输入登录密码!'
          return
        }
        this.login()
      },
      login() {
        let _this = this
        let data = {
          userName: this.params.userCode,
          password: this.params.password,
          ClientInfo: 'NZClient',
          remember: true
        }
        axios({
          host: 'userApi',
          type: 'post',
          url: 'login',
          loading: true,
          data
        }).then(res => {
          setCookie('USER_LOGIN_NAME', res.data.Token)
          _this.$router.push('/yunQuestion')
        })
      }
    }
  }
</script>
<style scoped>
  .home {
    position: relative;
    width: 100%;
    height: 100%;
  }
  .content {
    position: absolute;
    background: #f4f4f4;
    top: 56px;
    left: 0;
    right: 0;
    bottom: 0;
    box-sizing: border-box;
    overflow: auto;
    border: 1px red solid;
  }
</style>
