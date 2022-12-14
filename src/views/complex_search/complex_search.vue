<!-- eslint-disable no-unused-vars -->
<!-- eslint-disable vue/singleline-html-element-content-newline -->
<!-- eslint-disable vue/max-attributes-per-line -->
<!-- eslint-disable vue/require-v-for-key -->
<template>
  <div class="app-container">
    <el-row>
      <el-col :span="12">
        <el-form ref="form" :model="form" label-width="120px" style="padding-top:5vh">
          <el-form-item label="电影名称">
            <el-autocomplete
                v-model="form.title"
                :fetch-suggestions="movieSearchSuggest"
                placeholder="请输入内容"
                style="width: 20vw;"
                clearable
                @select="handleSelect"
                size="small"
              />
          </el-form-item>
          <el-form-item label="电影类别">
            <el-select v-model="form.genre" class="m-2" 
                       placeholder="请选择电影类别" style="width: 20vw" size="small">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="上映时间">
          </el-form-item>
          <el-form-item label="导演">
            <el-autocomplete
              v-model="form.director"
              :fetch-suggestions="directorSearchSuggest"
              placeholder="请输入导演名"
              style="width: 20vw;"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="演员">
            <el-autocomplete
              v-model="form.actor"
              :fetch-suggestions="movieSearchSuggest"
              placeholder="请输入演员名"
              style="width: 20vw;"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="评分">
            <el-input-number
              v-model="form.score.min"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="5"
              :min="0"
            />
            <span style="margin-left:10px;margin-right:10px">至</span>
            <el-input-number
              v-model="form.score.max"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="5.0"
              :min="movieMinScore"
            />
          </el-form-item>
          <el-form-item label="查询字段">
            <el-checkbox-group v-model="form.columns">
              <el-checkbox label="title">
                标题
              </el-checkbox>
              <el-checkbox label="asin">
                编号
              </el-checkbox>
              <el-checkbox label="score">
                评分
              </el-checkbox>
              <el-checkbox label="edition">
                版本
              </el-checkbox>
              <el-checkbox label="format">
                格式
              </el-checkbox>
              <el-checkbox label="date">
                日期
              </el-checkbox>
              <el-checkbox label="directors">
                导演
              </el-checkbox>
              <el-checkbox label="actors">
                演员
              </el-checkbox>
            </el-checkbox-group>
          </el-form-item>
        </el-form>
        <div style="text-align:center">
          <el-button type="primary" size="small" style="margin-left:3vh" plain>查询</el-button>
        </div>
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
  name: "ComplexSearch",
  data() {
    return {
      movieCategory:[],
      form:{
        title:"",
        score:{
          min:0,
          max:5.0,
        },
        date: {
          year: "",
          month: "",
          season: "",
          day: "",
          weekday: ""
        },
        title:"",
        genre:"",
        director:"",
        actor:"",
        columns:[],
      },
      test:"",
    };
  },

  methods: {
    handleSelect(item) {
      console.log(item);
    },
    movieSearchSuggest(queryString, cb){//电影搜索建议，需修改
      var result=[];
      cb(result);
    },
    directorSearchSuggest(queryString, cb){//导演搜索建议
      var result=[];
      cb(result);
    },
    actorSearchSuggest(queryString, cb){//演员搜索建议
      var result=[];
      cb(result);
    },
  },
};
</script>

<style scoped>
  .el-divider--vertical{
    height:75vh;
  }
</style>
