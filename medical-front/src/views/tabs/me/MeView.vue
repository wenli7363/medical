<script setup>

import {ref} from 'vue';
import {userLogout} from '@/api/authApi.js';
import {showNotify} from 'vant';
import {useRouter} from 'vue-router';
import {useUserStore} from '@/stores/userStore.js';

const router = useRouter();

const userStore = useUserStore();
const user = userStore.userInfo;

// // 用户信息
// const user = ref({
//   account: 'admin',
//   avatar: 'https://img.yzcdn.cn/vant/cat.jpeg',
//   collectionNumber: 10,
//   likeNumber: 20,
//   score: 100,
//   couponNumber: 5,
//   orderInfo: {
//     paidNumber: 10,
//     receivedNumber: 20,
//     shippedNumber: 30,
//     finishedNumber: 40
//   }
// })

const tools = ref([
  {
    label:'My Registration',
    path:'/myReg'
  },
  {
    label:'My Consultation',
    path:''
  },
  {
    label:'Family Record',
    path:''
  },
  {
    label:'My Comment',
    path:''
  },
  {
    label:'Official Service',
    path:''
  },

])

const onLogout = async () => {
  try {
    // 调用后端接口注销
    await userLogout();

    // 清除本地存储的用户信息
    localStorage.removeItem('token');
    localStorage.removeItem('USER_INFO');

    // 提示用户退出成功
    showNotify({ type: 'success', message: 'Logout successful' });

    // 跳转到登录页面
    router.replace('/login');
  } catch (error) {
    // 提示用户退出失败
    showNotify({ type: 'danger', message: 'Logout failed, please try again' });
    console.error('Logout failed:', error);
  }
};


import mine1 from '@/assets/img/navi/navigator-icon-1.png'
import mine2 from '@/assets/img/navi/navigator-icon-2.png'
import mine3 from '@/assets/img/navi/navigator-icon-3.png'
import mine4 from '@/assets/img/navi/navigator-icon-4.png'
import CpIcon from '@/components/CpIcon.vue';

</script>


<template>
  <div class="user-page">
    <div class="user-page-head">
      <div class="top">
        <van-image round fit="cover" :src="user?.avatar" />
        <div class="name">
          <p>{{ user.account }}</p>
          <p><van-icon name="edit" /></p>
        </div>
      </div>
      <van-row>
        <van-col span="6">
          <p>{{ user.collectionNumber }}</p>
          <p>favorites</p>
        </van-col>
        <van-col span="6">
          <p>{{ user.likeNumber }}</p>
          <p>focus</p>
        </van-col>
        <van-col span="6">
          <p>{{ user.score }}</p>
          <p>credit</p>
        </van-col>
        <van-col span="6">
          <p>{{ user.couponNumber }}</p>
          <p>coupon</p>
        </van-col>
      </van-row>
    </div>

    <div class="bg-white mb-4 flex rounded p-4 justify-between">
      <div class="tool-item">
        <img class="tool-icon" :src="mine1" alt="">
        <span class="tool-text">Real name registration</span>
      </div>
      <div class="tool-item">
        <img class="tool-icon" :src="mine2" alt="">
        <span class="tool-text">My doctor</span>
      </div>
      <div class="tool-item">
        <img class="tool-icon" :src="mine3" alt="">
        <span class="tool-text">Inspection report</span>
      </div>
      <div class="tool-item">
        <img class="tool-icon" :src="mine4" alt="">
        <span class="tool-text">Electronic prescription</span>
      </div>
    </div>

    <div class="user-page-group">
      <h3>Quick Tools</h3>
      <van-cell
          v-for="(item, i) in tools"
          :key="item.label"
          :title="item.label"
          :to="item.path"
          is-link
          :border="false"
      >
        <template #icon><cp-icon :name="`user-tool-0${i + 1}`" /></template>
      </van-cell>
    </div>
    <!-- 退出登录 -->
    <a href="javascript:;" class="logout" @click="onLogout">Logout</a>
  </div>
</template>


<style lang="scss" scoped>
.user-page {
  background-color: #F6F7F9;
  min-height: calc(100vh - 50px);
  padding: 0 15px 65px;
  // 头部
  &-head {
    height: 200px;
    background: linear-gradient(
            180deg,
            rgba(44, 181, 165, 0.46),
            rgba(44, 181, 165, 0)
    );
    margin: 0 -15px;
    padding: 0 15px;
    .top {
      display: flex;
      padding-top: 50px;
      align-items: center;
      .van-image {
        width: 70px;
        height: 70px;
      }
      .name {
        padding-left: 10px;
        p {
          &:first-child {
            font-size: 18px;
            font-weight: 500;
          }
          &:last-child {
            margin-top: 10px;
            color: var(--cp-primary);
            font-size: 16px;
          }
        }
      }
    }
    .van-row {
      margin: 0 -15px;
      padding-top: 15px;
      p {
        text-align: center;
        &:first-child {
          font-size: 18px;
          font-weight: 500;
        }
        &:last-child {
          color:  #979797;
          font-size: 16px;
          padding-top: 4px;
        }
      }
    }
  }
  // 订单
  &-order {
    background-color: #fff;
    border-radius: 8px;
    margin-bottom: 15px;
    padding-bottom: 15px;
    .head {
      display: flex;
      justify-content: space-between;
      line-height: 50px;
      padding: 0 15px;
      a {
        color: #C3C3C5;
      }
    }
    .van-col {
      text-align: center;
      .cp-icon {
        font-size: 28px;
      }
      p {
        padding-top: 4px;
        font-size: 12px;
      }
    }
  }
  // 分组
  &-group {
    background-color: #fff;
    border-radius: 8px;
    overflow: hidden;
    h3 {
      padding-left: 16px;
      line-height: 44px;
    }
    .van-cell {
      align-items: center;
    }
    .cp-icon {
      font-size: 17px;
      margin-right: 10px;
    }
  }
  .logout {
    display: block;
    margin: 20px auto;
    width: 100px;
    text-align: center;
    color: #EB5757;
  }
}

.bg-white {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;

  .tool-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    flex: 1; // 确保每个工具项均匀分布

    .tool-icon {
      width: 60px; // 统一图标大小
      height: 60px;
      margin-bottom: 8px; // 图标与文字间距
    }

    .tool-text {
      font-size: 14px; // 统一文字大小
      color: #666; // 文字颜色
    }
  }
}
</style>


