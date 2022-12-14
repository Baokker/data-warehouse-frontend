<template>
  <el-button @click.native="test()">点击发送</el-button>
</template>

<script>
export default {
  // eslint-disable-next-line vue/name-property-casing
  name: "test",
  data() {
    return {};
  },
  methods: {
    test() {
      this.$axios.get("/spark/bytime/year", { params: { year: 2008 } });
      this.$axios.get("/mysql/bytime/year", { params: { year: 2008 } });
      this.$axios.get("/neo4j/bytime/year", { params: { year: 2008 } });
    },
  },
};
</script>
