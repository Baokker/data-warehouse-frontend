<!-- eslint-disable no-unused-vars -->
<!-- eslint-disable vue/singleline-html-element-content-newline -->
<!-- eslint-disable vue/max-attributes-per-line -->
<!-- eslint-disable vue/require-v-for-key -->
<template>
  <div class="app-container">
    <el-row>
      <el-col :span="12">
        <el-form
          ref="form"
          :model="form"
          label-width="120px"
          style="padding-top: 5vh"
        >
          <el-form-item label="电影名称">
            <el-autocomplete
              v-model="form.title"
              :fetch-suggestions="movieSearchSuggest"
              placeholder="请输入电影名"
              style="width: 20vw"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="电影类别">
            <el-autocomplete
              v-model="form.genre"
              :fetch-suggestions="genreSearchSuggest"
              placeholder="请输入电影类别"
              style="width: 20vw"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="上映时间">
            <div>
              <span
                style="font-weight: 300; margin-left: 5px; margin-right: 10px"
                >年份（1931-2022）</span
              >
              <el-input-number
                v-model="form.year"
                size="mini"
                :max="2022"
                :min="1930"
                controls-position="right"
                style="width: 146px"
              />
            </div>
            <div style="margin-top: 10px">
              <span
                style="font-weight: 300; margin-left: 5px; margin-right: 20px"
                >月份或季度</span
              >
              <el-select
                v-model="form.month_season"
                class="m-2"
                placeholder="Select"
                size="small"
                style="width: 75px; margin-right: 15px"
              >
                <el-option label="月份" value="month"></el-option>
                <el-option label="季度" value="season"></el-option>
                <el-option label="无" value=""></el-option>
              </el-select>
              <el-input-number
                v-model="form.month"
                v-if="form.month_season == 'month'"
                size="mini"
                :max="12"
                :min="1"
                controls-position="right"
                style="width: 100px"
              />
              <el-input-number
                v-model="form.season"
                v-if="form.month_season == 'season'"
                size="mini"
                :min="1"
                :max="4"
                controls-position="right"
                style="width: 100px"
              />
            </div>
            <div style="margin-top: 10px">
              <span
                style="font-weight: 300; margin-left: 5px; margin-right: 20px"
                >天数或周几</span
              >
              <el-select
                v-model="form.day_weekday"
                class="m-2"
                placeholder="Select"
                size="small"
                style="width: 75px; margin-right: 15px"
              >
                <el-option label="天数" value="day"></el-option>
                <el-option label="周几" value="weekday"></el-option>
                <el-option label="无" value=""></el-option>
              </el-select>
              <el-input-number
                v-model="form.day"
                v-if="form.day_weekday == 'day'"
                size="mini"
                :min="1"
                :max="31"
                controls-position="right"
                style="width: 100px"
              />
              <el-input-number
                v-model="form.weekday"
                v-if="form.day_weekday == 'weekday'"
                size="mini"
                :min="1"
                :max="7"
                controls-position="right"
                style="width: 100px"
              />
            </div>
          </el-form-item>
          <el-form-item label="导演">
            <el-autocomplete
              v-model="form.director"
              :fetch-suggestions="directorSearchSuggest"
              placeholder="请输入导演名"
              style="width: 20vw"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="演员">
            <el-autocomplete
              v-model="form.actor"
              :fetch-suggestions="actorSearchSuggest"
              placeholder="请输入演员名"
              style="width: 20vw"
              clearable
              @select="handleSelect"
              size="small"
            />
          </el-form-item>
          <el-form-item label="评分">
            <el-input-number
              v-model="form.min_score"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="form.max_score"
              :min="0.0"
            />
            <span style="margin-left: 10px; margin-right: 10px">至</span>
            <el-input-number
              v-model="form.max_score"
              size="mini"
              :precision="2"
              :step="0.01"
              :max="5.0"
              :min="form.min_score"
            />
          </el-form-item>
          <el-form-item label="查询字段">
            <el-checkbox-group v-model="form.columns">
              <el-checkbox label="title"> 标题 </el-checkbox>
              <el-checkbox label="asin"> 编号 </el-checkbox>
              <el-checkbox label="score"> 评分 </el-checkbox>
              <el-checkbox label="edition"> 版本 </el-checkbox>
              <el-checkbox label="format"> 格式 </el-checkbox>
              <el-checkbox label="date"> 日期 </el-checkbox>
              <el-checkbox label="directors"> 导演 </el-checkbox>
              <el-checkbox label="actors"> 演员 </el-checkbox>
            </el-checkbox-group>
          </el-form-item>
        </el-form>
        <div style="text-align: center">
          <el-button
            type="primary"
            @click="search(form)"
            size="small"
            style="margin-left: 3vh"
            plain
            >查询</el-button
          >
        </div>
      </el-col>
      <el-col :span="1">
        <el-divider direction="vertical" />
      </el-col>
      <el-col :span="10">
        <el-tabs v-model="activeName" @tab-click="handleClick">
          <el-tab-pane label="查询结果" name="search_res">
            <el-table
              :data="result"
              v-loading="isLoading"
              element-loading-text="正在为您查询..."
              stripe
              style="width: 100%"
              height="450"
            >
              <el-table-column
                prop="title"
                label="标题"
                width="140"
                v-if="columns.title"
              />
              <el-table-column
                prop="date"
                label="上映日期"
                width="100"
                v-if="columns.date"
              />
              <el-table-column
                prop="score"
                label="评分"
                width="100"
                v-if="columns.score"
              />
              <el-table-column
                prop="edition"
                label="版本"
                width="60"
                v-if="columns.edition"
              />
              <el-table-column
                prop="asin"
                label="编号"
                width="115"
                v-if="columns.asin"
              >
                <template slot-scope="scope">
                  <div v-for="(item, index) in scope.row.asin" :key="index">
                    <a :href="`https://www.amazon.com/dp/${item}`">{{ item }}</a>
                  </div>
                </template>
              </el-table-column>
              <el-table-column
                prop="formats"
                label="格式"
                width="140"
                v-if="columns.format"
              >
                <template slot-scope="scope">
                  <div v-for="(item, index) in scope.row.formats" :key="index">
                    {{ item }}
                  </div>
                </template>
              </el-table-column>
              <el-table-column
                prop="actors"
                label="演员"
                width="160"
                v-if="columns.actors"
              >
                <template slot-scope="scope">
                  <div v-for="(item, index) in scope.row.actors" :key="index">
                    {{ item }}
                  </div>
                </template>
              </el-table-column>
              <el-table-column
                prop="directors"
                label="导演"
                width="140"
                v-if="columns.directors"
              >
                <template slot-scope="scope">
                  <div
                    v-for="(item, index) in scope.row.directors"
                    :key="index"
                  >
                    {{ item }}
                  </div>
                </template>
              </el-table-column>
            </el-table>
            <el-row style="text-align: center; margin-top: 20px">
              <el-pagination
                layout="prev, pager, next, jumper"
                :current-page="currentPage"
                :page-size="10"
                :total="totalNum"
                @current-change="getNewPage(form)"
                small
              />
            </el-row>
          </el-tab-pane>
          <el-tab-pane label="速度对比" name="speed">
            <!-- mysql速度：{{ mysql_speed}}
            spark速度：{{ spark_speed}}<br>
            下面是echart -->
            <div id="speed" style="width: 400px; height: 400px"></div>
          </el-tab-pane>
        </el-tabs>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "ComplexSearch",
  data() {
    return {
      activeName: "search_res",
      isLoading: false,
      totalNum: 0,
      currentPage: 1,
      mysql_speed: 0,
      spark_speed: 0,
      form: {
        title: "",
        min_score: 0,
        max_score: 5.0,
        year: null,
        month: null,
        season: null,
        day: null,
        weekday: null,
        title: "",
        genre: "",
        director: "",
        actor: "",
        columns: [],
        month_season: "",
        day_weekday: "",
      },
      columns: {
        title: false,
        asin: false,
        score: false,
        edition: false,
        format: false,
        data: false,
        directors: false,
        actors: false,
      },
      result: [],
      test: "",
    };
  },

  mounted() {
    this.echartsInit();
  },

  watch: {
    //监听速度变化，重新渲染页面
    mysql_speed: {
      handler(newValue, oldValue) {
        this.mysql_speed = newValue;
        this.echartsInit();
      },
    },
    spark_speed: {
      handler(newValue, oldValue) {
        this.spark_speed = newValue;
        this.echartsInit();
      },
    },
  },

  methods: {
    handleSelect(item) {
      console.log(item);
    },

    handleClick(tab, event) {
      console.log(tab, event);
    },

    movieSearchSuggest(queryString, cb) {
      //电影搜索建议
      this.$axios
        .get("/mysql/suggest/movie", {
          params: {
            title: queryString,
            amount: 10,
          },
        })
        .then((res) => {
          console.log("这是结果", res.suggestions.length);
          console.log("这是第一条", res.suggestions[0]);
          var result = [];
          for (var i = 0; i < res.suggestions.length; i++) {
            result.push({ value: res.suggestions[i] });
          }
          console.log("这是result", result);
          cb(result);
        })
        .catch((err) => {
          this.$message.error("当前网络异常，请稍后再试");
        });
    },

    genreSearchSuggest(queryString, cb) {
      //类型搜索建议
      this.$axios
        .get("/mysql/suggest/genre", {
          params: {
            genre: queryString,
            amount: 10,
          },
        })
        .then((res) => {
          console.log(res);
          var result = [];
          for (var i = 0; i < res.suggestions.length; i++) {
            result.push({ value: res.suggestions[i] });
          }
          console.log("这是result", result);
          cb(result);
        })
        .catch((err) => {
          this.$message.error("当前网络异常，请稍后再试");
        });
    },

    directorSearchSuggest(queryString, cb) {
      //导演搜索建议
      this.$axios
        .get("/mysql/suggest/director", {
          params: {
            director: queryString,
            amount: 10,
          },
        })
        .then((res) => {
          console.log(res);
          var result = [];
          for (var i = 0; i < res.suggestions.length; i++) {
            result.push({ value: res.suggestions[i] });
          }
          console.log("这是result", result);
          cb(result);
        })
        .catch((err) => {
          this.$message.error("当前网络异常，请稍后再试");
        });
    },

    actorSearchSuggest(queryString, cb) {
      //演员搜索建议
      this.$axios
        .get("/mysql/suggest/actor", {
          params: {
            actor: queryString,
            amount: 10,
          },
        })
        .then((res) => {
          console.log(res);
          var result = [];
          for (var i = 0; i < res.suggestions.length; i++) {
            result.push({ value: res.suggestions[i] });
          }
          console.log("这是result", result);
          cb(result);
        })
        .catch((err) => {
          this.$message.error("当前网络异常，请稍后再试");
        });
    },

    search(form) {
      this.isLoading = true;
      if (form.columns.length == 0) {
        this.$message.warning("请至少输入一个条件!");
      } else {
        //判断年份是否为空
        if (form.year == 1930) {
          console.log("year为空");
          form.year = null;
          console.log(form.year);
        } else {
          console.log(form.year);
        }

        //判断月份或季度
        if (form.month_season == "") {
          console.log("月份季度为空");
          form.month = null;
          form.season = null;
        } else if (form.month_season == "month") {
          console.log("选择月份");
          form.season = null;
        } else if ((form.month_season = "season")) {
          console.log("选择季度");
          form.month = null;
        }

        //判断天数或星期几
        if (form.day_weekday == "") {
          console.log("天数为空");
          form.day = null;
          form.weekday = null;
        } else if (form.day_weekday == "day") {
          console.log("选择天数");
          form.weekday = null;
        } else if ((form.day_weekday = "weekday")) {
          console.log("选择周几");
          form.day = null;
        }

        for (var i in this.columns) {
          //清空原有条件
          this.columns[i] = false;
        }

        console.log("这是原始条件", form.columns);
        for (var i = 0; i < form.columns.length; i++) {
          this.columns[form.columns[i]] = true;
          console.log(
            "这是条件",
            form.columns[i],
            this.columns[form.columns[i]]
          );
        }

        // //调用mysql查询电影总数
        // this.$axios.post("mysql/count/movie", {
        //     genre_name:form.genre,
        //     min_score:form.min_score,
        //     max_score:form.max_score,
        //     title:form.title,
        //     director:form.director,
        //     actor:form.actor,
        //     year:form.year,
        //     month:form.month,
        //     season:form.season,
        //     weekday:form.weekday,
        //     day:form.day,
        //     page:1,
        //     per_page:10,
        // }).then(res => {
        //   console.log("mysql总数",res.pages);
        //   this.isLoading=false;
        // }).catch(err => {
        //   this.$message.error('当前mysql网络异常，请稍后再试');
        // });

        //调用mysql查询总数
        this.$axios
          .post("/mysql/count/movie", {
            genre_name: form.genre,
            min_score: form.min_score,
            max_score: form.max_score,
            title: form.title,
            director: form.director,
            actor: form.actor,
            year: form.year,
            month: form.month,
            season: form.season,
            weekday: form.weekday,
            day: form.day,
            page: 1,
            per_page: 10,
          })
          .then((res) => {
            console.log("pages:", res.pages);
          });

        //调用mysql查询
        this.$axios
          .post("/mysql/comprehensive/movie", {
            genre_name: form.genre,
            min_score: form.min_score,
            max_score: form.max_score,
            columns: form.columns,
            title: form.title,
            director: form.director,
            actor: form.actor,
            year: form.year,
            month: form.month,
            season: form.season,
            weekday: form.weekday,
            day: form.day,
            page: 1,
            per_page: 10,
          })
          .then((res) => {
            console.log("这是mysql的结果", res);
            console.log("data", res.data);
            this.result = res.data;
            console.log(this.result);
            if (this.columns["title"] == true) {
              for (var i = 0; i < this.result.length; i++) {
                console.log("标题", this.result[i].title);
                this.result[i].title = this.result[i].title.replace(/\"/g, "");
                console.log("标题后", this.result[i].title);
              }
            }
            this.isLoading = false;
            this.mysql_speed = res.consuming_time;
            console.log("mysql速度", this.mysql_speed);
          })
          .catch((err) => {
            this.$message.error("当前mysql网络异常，请稍后再试");
          });

        //调用hive和spark查询
        this.$axios
          .post("/spark/comprehensive/movie", {
            genre_name: form.genre,
            min_score: form.min_score,
            max_score: form.max_score,
            columns: form.columns,
            title: form.title,
            director: form.director,
            actor: form.actor,
            year: form.year,
            month: form.month,
            season: form.season,
            weekday: form.weekday,
            day: form.day,
            page: 1,
            per_page: 10,
          })
          .then((res) => {
            console.log("这是spark的结果", res);
            this.totalNum = res.count;
            this.spark_speed = res.consuming_time;
          })
          .catch((err) => {
            this.$message.error("当前spark网络异常，请稍后再试");
          });
      }
    },

    getNewPage(form) {
      console.log("切换页面");
      console.log("当前页数", this.currentPage);
      this.isLoading = true;
      if (form.columns.length == 0) {
        this.$message.warning("请至少输入一个条件!");
      } else {
        //判断年份是否为空
        if (form.year == 1930) {
          console.log("year为空");
          form.year = null;
          console.log(form.year);
        } else {
          console.log(form.year);
        }

        //判断月份或季度
        if (form.month_season == "") {
          console.log("月份季度为空");
          form.month = null;
          form.season = null;
        } else if (form.month_season == "month") {
          console.log("选择月份");
          form.season = null;
        } else if ((form.month_season = "season")) {
          console.log("选择季度");
          form.month = null;
        }

        //判断天数或星期几
        if (form.day_weekday == "") {
          console.log("天数为空");
          form.day = null;
          form.weekday = null;
        } else if (form.day_weekday == "day") {
          console.log("选择天数");
          form.weekday = null;
        } else if ((form.day_weekday = "weekday")) {
          console.log("选择周几");
          form.day = null;
        }
        console.log("这是原始条件", form.columns);
        for (var i = 0; i < form.columns.length; i++) {
          this.columns[form.columns[i]] = true;
          console.log(
            "这是条件",
            form.columns[i],
            this.columns[form.columns[i]]
          );
        }

        //调用mysql查询
        this.$axios
          .post("/mysql/comprehensive/movie", {
            genre_name: form.genre,
            min_score: form.min_score,
            max_score: form.max_score,
            columns: form.columns,
            title: form.title,
            director: form.director,
            actor: form.actor,
            year: form.year,
            month: form.month,
            season: form.season,
            weekday: form.weekday,
            day: form.day,
            page: this.currentPage,
            per_page: 10,
          })
          .then((res) => {
            console.log("这是mysql的结果", res);
            console.log("data", res.data);
            this.result = res.data;
            console.log(this.result);
            this.isLoading = false;
          })
          .catch((err) => {
            this.$message.error("当前mysql网络异常，请稍后再试");
          });
      }
    },

    echartsInit() {
      //使用时只需要把setOption里的对象换成echarts中的options或者自己的参数即可
      console.log("开始初始化");
      this.$echarts.init(document.getElementById("speed")).setOption({
        title: {
          text: "综合查询速度对比",
        },
        tooltip: {},
        xAxis: {
          data: ["mysql", "spark"],
        },
        yAxis: {},
        series: [
          {
            name: "查询耗时",
            type: "bar",
            data: [this.mysql_speed, this.spark_speed],
          },
        ],
      });
    },
  },
};
</script>

<style scoped>
.el-divider--vertical {
  height: 100vh;
}
</style>
