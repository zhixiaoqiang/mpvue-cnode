<template>
  <div class='container' @click.stop="goDetail($event)">
    <div class='head'>
      <img class='head-img' :src='item.author.avatar_url' :data-loginname='item.author.loginname' @click.stop='goAuthorPage'>
      <div class='info'>
        <span>{{item.author.loginname}}</span>
        <span class='time' v-if='!hidden'>{{formatCreateAt}}</span>
      </div>
    </div>
    <div class='body'>
      <p>{{item.title}}</p>
    </div>
    <div class='foot'>
      <div class='count' v-if='!hidden'>
        <span class='number'>回答：{{item.reply_count}}</span>
        <span class='number'>浏览：{{item.visit_count}}</span>
      </div>
      <div class='later'>
        <span class='number'>最新动态：{{formatLastReply}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { passTime } from "../utils/index";
export default {
  props: {
    item: Object,
    hidden: {
      // 个人中心展示帖子隐藏一部分东西的复用
      default: false,
      type: Boolean
    }
  },
  computed: {
    formatLastReply() {
      return passTime(this.item.last_reply_at);
    },
    formatCreateAt() {
      return passTime(this.item.create_at);
    }
  },
  methods: {
    goAuthorPage(e) {
      // e.currentTarget.dataset.author
      console.log(e);
      wx.setStorageSync("loginname", e.currentTarget.dataset.loginname);
      wx.navigateTo({ url: "../user/main" });
    },
    goDetail() {
      wx.setStorageSync("topicid", this.item.id);
      wx.navigateTo({
        url: "../detail/main"
      });
    }
  }
};
</script>

<style scoped>
.container {
  height: 330rpx;
  padding: 30rpx;
  margin-bottom: 20rpx;
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.head {
  color: rgb(65, 184, 131);
  display: flex;
  align-items: center;
}
.head-img {
  width: 64rpx;
  height: 64rpx;
}
.info {
  flex-direction: column;
  display: flex;
  margin-left: 26rpx;
}
.time {
  color: #888;
}
.body {
  color: rgb(65, 184, 131);
}
.body > p {
  font-weight: bold;
}
.number {
  color: #888;
  font-size: 26rpx;
}
.number + .number {
  margin-left: 20rpx;
}
.foot {
  display: flex;
  justify-content: space-between;
}
</style>
