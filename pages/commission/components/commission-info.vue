<!-- 分销商信息  -->
<template>
  <!-- 用户资料 -->
  <view class="user-card ss-flex ss-col-bottom">
    <view class="card-top ss-flex ss-row-between">
      <view class="ss-flex">
        <view class="head-img-box">
          <image class="head-img" :src="sheep.$url.cdn(userInfo.avatar)" mode="aspectFill"></image>
        </view>
        <view class="ss-flex-col">
          <view class="user-name">{{ userInfo.nickname }}</view>
          <view class="user-info-box ss-flex">
            <view class="tag-box ss-flex" v-if="agentInfo.level_info">
              <image
                v-if="agentInfo.level_info?.image"
                class="tag-img"
                :src="sheep.$url.cdn(agentInfo.level_info?.image)"
                mode="aspectFill"
              >
              </image>
              <text class="tag-title">{{ agentInfo.level_info?.name }}</text>
            </view>
            <view class="ss-iconfont uicon-arrow-right" style="color: #fff; font-size: 28rpx">
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup>
  import sheep from '@/sheep';
  import { computed, reactive } from 'vue';

  const userInfo = computed(() => sheep.$store('user').userInfo);
  const agentInfo = computed(() => sheep.$store('user').agentInfo);
  const headerBg = sheep.$url.css('/assets/addons/shopro/uniapp/commission/background.png');

  const state = reactive({
    showMoney: false,
  });
</script>

<style lang="scss" scoped>
  // 用户资料卡片
  .user-card {
    width: 690rpx;
    height: 192rpx;
    margin: -88rpx 20rpx 0 20rpx;
    padding-top: 88rpx;
    background: v-bind(headerBg) no-repeat;
    background-size: 100% 100%;

    .head-img-box {
      margin-right: 20rpx;
      width: 100rpx;
      height: 100rpx;
      border-radius: 50%;
      position: relative;
      background: #fce0ad;

      .head-img {
        width: 92rpx;
        height: 92rpx;
        border-radius: 50%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    }

    .card-top {
      box-sizing: border-box;
      padding-bottom: 34rpx;
      .user-name {
        font-size: 32rpx;
        font-weight: bold;
        color: #692e04;
        line-height: 30rpx;
        margin-bottom: 20rpx;
      }

      .log-btn {
        width: 84rpx;
        height: 42rpx;
        border: 2rpx solid rgba(#ffffff, 0.33);
        border-radius: 21rpx;
        font-size: 22rpx;
        font-weight: 400;
        color: #ffffff;
        margin-bottom: 20rpx;
      }

      .look-btn {
        color: #fff;
        width: 40rpx;
        height: 40rpx;
      }
    }

    .user-info-box {
      .tag-box {
        background: #ff6000;
        border-radius: 18rpx;
        line-height: 36rpx;

        .tag-img {
          width: 36rpx;
          height: 36rpx;
          border-radius: 50%;
          margin-left: -2rpx;
        }

        .tag-title {
          font-size: 24rpx;
          padding: 0 10rpx;
          font-weight: 500;
          line-height: 36rpx;
          color: #fff;
        }
      }
    }
  }
</style>
