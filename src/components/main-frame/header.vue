<template>
  <header class="frame-header">
    <el-button-group>
      <el-button
          type="text"
          @click="go(-1)">
        <i class="iconfont icon-arrow-back"></i>
      </el-button>
      <el-button
          type="text"
          @click="go(1)">
        <i class="iconfont icon-arrow-forward"></i>
      </el-button>
    </el-button-group>
    <div class="right"
        v-if="!loading">
      <img
          :src="images.avatar">
      <span>{{ profile.name }}</span>
      <el-button
          type="text"
          :disabled="sending"
          @click="logout">
        <i class="iconfont icon-power"></i>
      </el-button>
    </div>
  </header>
</template>

<script>
import UserAPI from '../../api/user';

import Storage from '../../utils/storage';

import avatar from '../../assets/avatar.png';

export default {
  name: 'FrameHeader',
  data() {
    return {
      loading: false,
      profile: {
        name: '',
        mail: '',
      },
      sending: false,
      images: {
        avatar,
      },
    };
  },
  mounted() {
    const hasLogin = Storage.getToken();
    if (hasLogin) {
      this.getProfile();
    }
  },
  methods: {
    getProfile() {
      if (this.loading) {
        return;
      }

      this.loading = true;

      this.$store.dispatch('getProfile')
        .then((data) => {
          this.profile = data;
        })
        .catch(() => {
          this.$message.error(this.$t('request-message.load-user-info-failed'));
        })
        .finally(() => {
          this.loading = false;
        });
    },
    go(steps) {
      this.$router.go(steps);
    },
    logout() {
      if (this.sending) {
        return;
      }

      this.sending = true;

      UserAPI.logOut()
        .then(() => {
          Storage.clearAuthInfo();

          this.$router.push({
            name: 'login',
          });
        })
        .finally(() => {
          this.sending = false;
        });
    },
  },
};
</script>

<style
    src="./header.scss"
    lang="scss"
    scoped></style>
