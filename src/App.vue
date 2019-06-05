<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>JWT</h1>
  </div>
</template>

<script>
import jsrsasign from "jsrsasign";
export default {
  name: "app",
  data() {
    return {
      number: "123",

      jwt_header: {
        alg: "HS256",
        typ: "JWT"
      },
      jwt_payLoad: {},
      JWT: ""
    };
  },
  mounted() {
    this.$nextTick(function() {
      this.setPayLoad();
      this.setJWT();
    });
  },
  methods: {
    /** 设置负载 */
    setPayLoad() {
      var tNow = new Date().getTime();
      var tEnd = tNow + 1000 * 60 * 60;
      this.jwt_payLoad = {
        sub: "forget",
        iat: tNow, // 签发时间
        nbf: tNow, // 生效时间
        exp: tEnd // 过期时间
      };
    },
    setJWT() {
      let header = JSON.stringify(this.jwt_header);
      let payload = JSON.stringify(this.jwt_payLoad);
      this.JWT = jsrsasign.KJUR.jws.JWS.sign(
        "HS256",
        header,
        payload,
        this.number
      );
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
