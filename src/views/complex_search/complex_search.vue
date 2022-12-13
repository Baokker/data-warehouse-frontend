<template>
  <div>
    <el-row>
      <el-col :span="12">
        <el-form ref="form" :model="form" label-width="120px" style="padding-top:5vh">
          <el-form-item label="电影名称">
            <el-autocomplete
                v-model="test"
                :fetch-suggestions="movieSearchSuggest"
                placeholder="请输入内容"
                style="width: 20vw;"
                clearable
                @select="handleSelect"
                size="small"
              />
          </el-form-item>
          <el-form-item label="电影类别">
            <el-select v-model="category" class="m-2" 
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
              v-model="test"
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
              v-model="test"
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
              v-model="movieMinScore"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="5"
              :min="0"
            />
            <span style="margin-left:10px;margin-right:10px">至</span>
            <el-input-number
              v-model="movieMaxScore"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="5.0"
              :min="movieMinScore"
            />
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
/* eslint-disable */
export default {
  filters: {

  },
  
  data() {
    return {
      movieMinScore:0,
      movieMaxScore:5.0,
      movieCategory:[],
      category:"",
      form1:{
        name1:"",
        name2:"",
        name3:"",
      },
      test:"",
      form: reactive({
          name: '',
          sudo:'',
        }),
    }
  },

  created(){
    console.log(this.form.name);

  },
  methods:{
    handleSelect(item) {
      console.log(item);
    },
    movieSearchSuggest(queryString, cb){//电影搜索建议，需修改
      var result=[];
      cb(result);
    },
  },
    directorSearchSuggest(queryString, cb){//导演搜索建议
      var result=[];
      cb(result);
    },
    actorSearchSuggest(queryString, cb){//演员搜索建议
      var result=[];
      cb(result);
    },
}
</script>

<style scoped>
  .el-divider--vertical{
    height:75vh;
  }
</style>