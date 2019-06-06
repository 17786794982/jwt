<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>JWT</h1>
    <div class="jwt">{{JWT}}</div>
  </div>
</template>

<script>
import jsrsasign from "jsrsasign";
export default {
  name: "app",
  data() {
    return {
      secret: "123", // 秘钥

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
      var tNow = jsrsasign.KJUR.jws.IntDate.getNow();
      // var tEnd = jsrsasign.KJUR.jws.IntDate.get('now + 1hour');
      var tEnd = Math.round((new Date().getTime() + 60000)/1000) 
      this.jwt_payLoad = {
        sub: "getCode",
        iss: '123457890', // 签发人
        iat: tNow, // 签发时间
        nbf: tNow, // 生效时间
        exp: tEnd, // 过期时间
        random: Math.random(),
      };
    },
    setJWT() {
      let header = JSON.stringify(this.jwt_header);
      let payload = JSON.stringify(this.jwt_payLoad);
      this.JWT = jsrsasign.KJUR.jws.JWS.sign(
        "HS256",
        header,
        payload,
        this.secret,
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

.jwt {
  width: 500px;
  min-height: 500px;
  text-align: left;
  margin: 0 auto;
  white-space: normal;
  word-break: break-all;
}
</style>
