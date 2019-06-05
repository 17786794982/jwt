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

      // let secret = 'MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBANESTe7LjH2LhrXo5g5gSnkivZ/XqWyZQcHYeMYOXGRTSO71gCnJ5mVRdvX3VmTEna/Hb68qmk3iAosPLmvskxOnByHUI29x7JJfoOIziXBMCdQRmIFiA0E2sog0S0mZdZJkFN5Hu/scf8TE0/m/KGGTTovWU6iSeFhyr30WNMaHAgMBAAECgYAE609WHQfzNEM5KH+xOubFruGTTzm3SmvXqcY9srzNx3/hz3jygsOfAqmv49/ugwnKxwCDuJbk2jqBFxxagbh8JPmMG8nNwyWhmcDfqwTRu3OVOP//vXigFiIRw7hpYLskHIyBVZnZZqSotAaiK/igp3OLOBzQuYILdczn4X5GkQJBAPjIdAm4l3r+OBcgTy/zcCJQkOHQ3yc63ZIapbNmlgozU5RAhX6yzdKRHPnJap9QEqDJTw5WRFaPvxG5mLZayKMCQQDXIvCj6muQGbmuBtAgZGcMnkFt//T+n59Il+ba2JDWj3hOO6Emzj16EUu0kqlBP3fGU6cXAjusAlZVR0s9SzTNAkAvjqIf+Zl7eX1fbl203ORiquQHRtZhuW8BrvZeBQ5JhOZFQNBEGAogZn0Tgt1O9w+YjOL/6p3FrlToHoKC2XfhAkEAipTPPkd7Ek//88Ifvz3tw4sNyrXeM0bPbAutgbuPUScJ8BspK74ei8soYSE4NfeUSAUK1R9zINJAmp5aMRmI4QJBAL29qAbT53Eua+VeDqxgQ3Vz54jwokhqkqDRcHM8Cphx9PZyEXd1Q8DehN+uTnUxsdoaMjVSR3vpWnZDzckKVuk=';

      // let prvKey = jsrsasign.KEYUTIL.getKey('sPKCS8PEM', this.number);
      // console.log(prvKey);
      this.JWT = jsrsasign.KJUR.jws.JWS.sign(
        "HS256",
        header,
        payload,
        this.number,
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

.jwt{
  width: 500px;
  min-height: 500px;
  text-align: left;
  margin: 0 auto;
  white-space: normal;
  word-break: break-all;
}
</style>
