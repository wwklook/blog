<template>
  <div>
    <headers />
    <div class="account">
      <div class="account-left">
        <div class="account-title">个人中心</div>
        <ul class="account-ul">
          <li
            class="account-list"
            :class="{ 'account-active': acc.name == this.$route.name }"
            v-for="acc in account"
            :key="acc.name"
            @click="nav(acc.name)"
          >
            {{ acc.text }}
          </li>
        </ul>
      </div>
      <div class="account-right">
        <div class="account-top">{{ text }}</div>
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Headers from "@/components/home/Headers.vue";
import { isLogin } from "@/network/info";

export default {
  name: "Account",
  components: {
    Headers,
  },
  computed: {
    text() {
      for (const item of this.account) {
        if (item.name == this.$route.name) {
          return item.text;
        }
      }
    },
  },
  mounted() {
    isLogin().then((res) => {
      if (res.data == "no") {
        location.href = "/login.html";
      }
    });
  },
  data() {
    return {
      account: [
        {
          name: "AccountIndex",
          text: "主页",
        },
        {
          name: "AccountInfo",
          text: "我的信息",
        },
        {
          name: "AccountFace",
          text: "我的头像",
        },
      ],
      userinfo: {},
    };
  },
  methods: {
    nav(name) {
      if (this.$route.name != name) {
        this.$router.push({ name });
      }
    },
  },
};
</script>

<style lang="sass" scoped>
.account
  width: 980px
  height: 100%
  margin: 100px auto
  border: 1px solid #e1e2e5
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.14)
  background: #fafafa
  border-radius: 4px
  display: flex


  &-left
    width: 150px
    overflow: hidden


  &-right
    width: 829px
    min-height: 890px
    border-left: 1px solid #ddd
    background: #fff


  &-top
    height: 49px
    color: #00aaff
    line-height: 49px
    padding-left: 20px
    border-bottom: 1px solid #e1e2e5


  &-title
    width: 150px
    height: 49px
    text-align: center
    line-height: 49px
    font-size: 16px
    color: #99a2aa
    border-bottom: 1px solid #e1e2e5


  &-ul
    padding: 0
    margin: 0
    border-bottom: 1px solid #e1e2e5


  &-list
    width: 100%
    height: 49px
    display: flex
    justify-content: center
    align-items: center
    list-style: none
    cursor: pointer


  &-list:hover
    background-color: #e2e2e2


  &-active
    background-color: #00aaff !important
    color: #ffffff
</style>