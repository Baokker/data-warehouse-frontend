<!-- eslint-disable no-unused-vars -->
<!-- eslint-disable vue/singleline-html-element-content-newline -->
<!-- eslint-disable vue/max-attributes-per-line -->
<!-- eslint-disable vue/require-v-for-key -->
<template>
  <div class="app-container">
    <el-dialog
      title="电影详情"
      :visible.sync="dialogVisible"
      width="25%"
      center
      @close="dialogVisible = false"
    >
      <div>
        <p>asin:{{ dialogData.asin }}</p>
        <p>电影名:{{ dialogData.name }}</p>
        <p v-if="dialogData.director.length !== 0">
          导演：
          <span v-for="i in dialogData.director">{{ i }}, </span>
        </p>
        <p v-if="dialogData.actor.length !== 0">
          演员：
          <span v-for="i in dialogData.actor">{{ i }}, </span>
        </p>
        <p>评分:{{ dialogData.score }}</p>
        <p>评论总数:{{ dialogData.commentNum }}</p>
      </div>
      <div slot="footer">
        <el-button type="primary" @click="viewOriginWeb()"
          >查看原始网页</el-button
        >
        <br /><br />
        <el-button @click="dialogVisible = false">关闭</el-button>
      </div>
    </el-dialog>

    <el-row style="height: 50vh">
      <el-col :span="12">
        <el-form>
          <el-form-item>
            <el-button round @click="button">合作次数最多的演员</el-button>
          </el-form-item>
          <el-form-item>
            <el-button round @click="mostCooperateActorAndDirectorButton"
              >合作次数最多的演员和导演</el-button
            >
          </el-form-item>
        </el-form>
      </el-col>
      <el-col :span="1">
        <el-divider direction="vertical" />
      </el-col>
      <el-col :span="10">
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="查询结果" name="first" />
          <el-tab-pane label="速度对比" name="third" />
        </el-tabs>
      </el-col>
    </el-row>
  </div>
</template>

<script>
const BASE_URL = "https://81.68.102.171:5000/api";

import axios from "axios";
export default {
  name: "RelationSearch",
  data() {
    return {
      dialogVisible: false,
      dialogData: {
        asin: "1234",
        name: "11",
        director: ["小明", "小红", "小黑", "小白"],
        actor: ["小蓝", "小黄", "小黑", "小白"],
        score: 2.5,
        commentNum: 100,
      },
      test:"嘻嘻",
    };
  },

  created() {
    console.log("test");
  },

  methods: {
    viewOriginWeb() {
      //跳转网页
      window.open("https:www.amazon.com/dp/" + this.dialogData.asin);
    },
    button() {
      this.$axios.post("/mysql/comprehensive/relation", {
        "source": "actor",
        "target": "actor",
        "times": "7",
        "name": "Oliver Hardy"
      }).then(res => {
        console.log(res)

      }).catch(err => {

      });
    },
  },
};
</script>

<style scoped>
  .el-divider--vertical{
    height:75vh;
  }
</style>
