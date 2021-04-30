<template>
  <div class="">
    用户名： <input type="text" v-model="username" /> 
  <br>
    <button @click="add">登录</button>
    {{ timers }}
  </div>
</template>

<script>
const reuser = /^[\da-zA-Z_\u4e00-\u9f5a]{6,18}$/;


export default {
  data() {
    return {
   
      
      username: "",
    };
  },
  mounted() {},
  methods: {
    add() {
      if (!reuser.test(this.username)) {
        alert("名称不规范");
      } else {
        this.$router.push({ path: "/about", query: { obj: this.username } });
      }
    },
  },
  components: {},
  computed: {
    show() {
      let price = 0;
      this.result.forEach((item) => {
        price += item.price * item.num;
      });
      return price;
    },
  },
};
</script>

<style lang="scss" scoped></style>
