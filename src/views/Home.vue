<template>
  <div class="home">
    <el-row :gutter="20">
      <el-col :span="3"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="18">
        <div class="grid-content bg-purple">
          <!-- 导入轮播图组件 -->
          <banner :banners="imgarr"></banner>
          <!-- 中间部分 -->
          <!-- 今日推荐 -->
          <div class="maincenter">
            <el-tabs v-model="activeName" @tab-click="handleClick">
              <el-tab-pane label="今日推荐" name="first">
                <ul>
                  <!-- 遍历接口数组 -->
                  <li
                    v-for="(item, index) in todayitem"
                    :key="index"
                    class="fl todayli"
                  >
                    <img :src="todayimg[index]" class="todayimg" />
                    <p>
                      {{ item.title | formatTitle("...") }}
                    </p>
                    <p>
                      票价：<span style="color: black">{{
                        item.price | formatPrice("￥")
                      }}</span>
                    </p>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="即将开售" name="second">
                <ul>
                  <!-- 遍历接口数组 -->
                  <li
                    v-for="(item, index) in todayitem"
                    :key="index"
                    class="fl todayli"
                  >
                    <img :src="todayimg[item.xiabiao]" class="todayimg" />
                    <p>
                      {{ item.title | formatTitle("...") }}
                    </p>
                    <p>
                      票价：<span style="color: black">{{
                        item.price | formatPrice("￥")
                      }}</span>
                    </p>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
          <!-- 中间部分结束 -->
          <div>
            <floorCommon></floorCommon>
          </div>
        </div>
      </el-col>
      <el-col :span="3"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
  </div>
</template>
<script>
import banner from "../components/banner.vue";
import floorCommon from '../components/floorCommon'

export default {
  components: {
    banner,
    floorCommon
  },
  data() {
    return {
      // 轮播图地址
      imgarr: ["img/base/pic1.jpg", "img/base/pic2.jpg", "img/base/pic3.jpg"],
      activeName: "first",
      // 今日推荐接口
      todayitem: [],
      // 今日推荐图片
      todayimg: [
        "img/base/cardimg1.jpg",
        "img/base/cardimg2.jpg",
        "img/base/cardimg3.jpg",
        "img/base/cardimg4.jpg",
        "img/base/cardimg5.jpg",
        "img/base/cardimg6.jpg",
      ],
    };
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    },
  },
  mounted() {
    this.$http("/api/home/todayitems").then((res) => {
      this.todayitem = res.data;
    });
  },
  // 题目、价格过滤器
  filters: {
    formatPrice: function (v, y) {
      return y + v;
    },
    formatTitle: function (v, p) {
      return v + p;
    },
  },
};
</script>
<style scoped>
.grid-content {
  min-height: 36px;
}
/* 中间部分 */
.maincenter {
  margin-top: 20px;
  width: 1359px;
  height: 280px;
  background-color: white;
  overflow: hidden;
  padding: 5px 0 5px 50px;
}
.todayimg {
  width: 150px;
  height: 170px;
}
.todayli p {
  color: gray;
  font-size: 14px;
}

/* 今日推荐列表 */
.todayli {
  margin-right: 75px;
}
</style>

<style>
/* 全局适应 */

</style>