<template>
  <section class="profile">
    <div class="page-loading loader-inner ball-pulse" v-if="isLoading">
      <div> </div> <div> </div> <div> </div>
    </div>
    <div class="user card" v-show="! isLoading">
      <div class="card-content">
        <div class="media">
          <figure class="media-left">
            <p class="image" style="height: 100px; width: 100px;">
              <img :src="profile.face_url">
            </p>
          </figure>
          <div class="media-content">
            <div class="content">
              <p class="title is-4"> <strong>{{ profile.id }}</strong> </p>
              <div class="items">
                <p class="subtitle is-6"> {{ profile.user_name }} </p>
                <p class="subtitle is-6">
                  <i class="iconfont role icon-account" aria-hidden="true"></i>
                  <span class="role">{{ profile.role }}</span>
                  <i v-if="profile.gender == 'm'" class="iconfont icon-male" aria-hidden="true"></i>
                  <i v-else class="iconfont icon-female" aria-hidden="true"></i>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="card-footer">
        <a class="card-footer-item">
          <div> {{ profile.life }}</div>
          <div>生命力</div>
        </a>
        <a class="card-footer-item">
          <div> {{ profile.post_count }} </div>
          <div> 文章 </div>
        </a>
        <a class="card-footer-item">
          <div> {{ profile.score }} </div>
          <div> 积分 </div>
        </a>
      </div>
    </div>

    <div class="pannel-box" v-show="! isLoading">
      <div class="mail box" @click="$router.push('/reply')">
        <i class="iconfont icon-remind"></i>&nbsp;
        <span> 回复我的 </span><span class="tag is-warning" v-if="profile.new_reply">{{ profile.new_reply }}</span>
      </div>
      <div class="reply box" @click="$router.push('/at')">
        <i class="iconfont icon-at"></i>&nbsp;
        <span> 提到我的 </span><span class="tag is-warning" v-if="profile.new_at">{{ profile.new_at }}</span>
      </div>
      <div class="refer box" @click="$router.push('/mail/inbox')">
        <i class="iconfont icon-email"></i>&nbsp;
        <span> 收件箱 </span>
        <span class="tag is-warning" v-if="profile.new_mail">新</span>
        <span class="tag is-danger" v-if="profile.full_mail">满</span>

      </div>
    </div>

    <div class="pannel-box infomations" v-show="! isLoading">
      <div class="box" @click="$router.push('/post?type=new&board=advice')">
        <i class="iconfont icon-edit"></i>&nbsp;
        <span>意见反馈</span>
      </div>
      <div class="box" @click="$router.push('/article/id/9272')">
        <i class="iconfont icon-attachment"></i>&nbsp;
        <span>论坛管理办法</span>
      </div>
      <div class="box" @click="$router.push('/contributor')">
        <i class="iconfont icon-smile"></i>&nbsp;
        <span> 特别感谢 </span>
      </div>
    </div>

    <div class="pannel-box" style="margin-bottom: 0" v-show="! isLoading">
      <div class="box" @click="getLogout">
        <span> 退出登录 </span>
      </div>
    </div>
  </section>
</template>

<script>

 import { mapGetters, mapActions } from 'vuex'
 import * as api from 'api/home'
 import * as authApi from 'api/auth'

 export default {
   data () {
     return {
       isLoading: true
     }
   },

   created() {
     this.fetchProfile();
   },

   computed: {
     ...mapGetters([
       'profile'
     ])
   },

   methods: {
     fetchProfile() {
       if (this.profile.isLogin) {
         return;
       }
       this.isLoading = true;
       api.getProfile().then((res) => {
         if (res.success) {
           this.login(res.data);
           this.isLoading = false;
         }
       });
     },

     getLogout() {
       authApi.logout().then((res) => {
         if (res.success) {
           this.logout()
           this.$router.push('/login')
         }
       })
     },

     ...mapActions([
       'login',
       'logout'
     ])
   }
 }
</script>
<style scoped>
 .tabs {
   margin-bottom: 8px;
 }
 span.role {
   padding-right: 8px;
   border-right: 1px solid #4a4a4a;
 }
 .card-footer {
 }
 .card-footer-item {
   border-right: 0;
   display: block;
   text-align: center;
 }
 .card-footer-item div {
   color: #333;
 }
 .pannel-box {
   margin: 16px 0;
   border-radius: 5px;
 }
 .pannel-box .box {
   margin-bottom: 0;
   border-radius: 0;
 }
 .pannel-box .iconfont {
   font-size: large;
 }
 img {
   width: auto;
 }

</style>
