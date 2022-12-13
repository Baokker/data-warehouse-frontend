<template>
  <el-button @click.native="test()">点击发送</el-button>
</template>

<script>
export default {
  // eslint-disable-next-line vue/name-property-casing
  name: "test",
  data() {
    return {
      BASE_URL: "http://81.68.102.171:5000/api/",
    };
  },
  methods: {
    test() {
      var axios = require("axios");

      var config = {
        method: "get",
        url: this.BASE_URL + "/bytime/year",
        params: {
          year: 2008,
        },
      };

      axios.interceptors.request.use((request) => {
        console.log(request);
        return request;
      });

      axios.interceptors.response.use((response) => {
        console.log(response);
        return response;
      });

      axios(config)
        .then((response) => {
          console.log(response.data);
        })
        // eslint-disable-next-line no-unused-vars
        .catch(function (error) {
          this.$message.error("当前网络异常，请稍后再试");
        });
    },
  },
};
</script>
