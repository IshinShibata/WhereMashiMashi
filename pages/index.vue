<template>
  <v-app>
    <v-col class="text-center" cols="13" sm="5">
      <v-btn color="blue" @click="getmashimashi" v-show="LatLongFlag">位置情報を送ってマシマシを探す</v-btn>
    </v-col>
    <v-layout>
      <v-flex xs6>
      <a href="https://api.gnavi.co.jp/api/scope/" target="_blank">
        <img src="https://api.gnavi.co.jp/api/img/credit/api_90_35.gif" width="90" height="35" border="0" alt="グルメ情報検索サイト　ぐるなび">
      </a>
      </v-flex>
    </v-layout>
  </v-app>
 
</template>

<script>
import axios from 'axios';

export default {
  components: {
  },
  data (){
    return {
      info: null,
      LatLongFlag: false,
      latlongs: ["", ""]
    }
  },
  computed:{
  },
  methods:{
    getmashimashi(){
       axios
      .get(`https://api.gnavi.co.jp/RestSearchAPI/v3/?keyid=${process.env.API_KEY}&category_l=RSFST08000&latitude=${this.latlongs[0]}&longitude=${this.latlongs[1]}&`)
      .then(response => (console.log(response.data.rest)))
    }
  },
  mounted (){
    if (navigator.geolocation) {
      this.LatLongFlag = true
    } else {
      this.LatLongFlag = false
    }
    navigator.geolocation.getCurrentPosition(
        (position) => {
          this.latlongs[0] = position.coords.latitude
          this.latlongs[1] = position.coords.longitude
          console.log(this.latlongs)
        },
        (error) => {
          switch(error.code) {
            case 1:
              alert("位置情報の利用が許可されていません");
              break;
            case 2:
              alert("現在位置が取得できませんでした");
              break;
            case 3:
              alert("タイムアウトになりました");
              break;
            default:
              alert("その他のエラー(エラーコード:"+error.code+")");
              break;
          }
        }
      )
  }
}
</script>