<template>
  <section class="contianer">
    <el-row type="flex" justify="space-between">
      <!-- 顶部过滤列表 -->
      <div class="flights-content">
        <!-- 过滤条件 -->
        <div>
          <section >
            <!-- 其他代码... -->

            <!-- 过滤条件 -->
            <!-- data 是不会被修改的列表数据 -->
            <!-- setDataList 用于修改过滤后的数组列表 -->
            <FlightsFilters :data="cacheFlightsData" />

            <!-- 其他代码... -->
          </section>
        </div>

        <!-- 航班头部布局 -->
        <div>
          <flightsListHead />
        </div>

        <!-- 航班信息 -->
        <div>
          <flightsItem v-for="(item,index) in dataList" :key="index" :data="item" />
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="pageIndex"
            :page-sizes="[5, 10, 15, 20]"
            :page-size="pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </div>
      </div>

      <!-- 侧边栏 -->
      <div class="aside">
        <!-- 侧边栏组件 -->
      </div>
    </el-row>
  </section>
</template>

<script>
import moment from "moment";
import flightsListHead from "@/components/air/flightsListHead";
import flightsItem from "@/components/air/flightsItem";
import FlightsFilters from '@/components/air/flightsFilters'

export default {
  data() {
    return {
      airlist: [],
      flightsData: {},
      // 当前显示的列表数组
      dataList: [],
    cacheFlightsData:{
      info:{},
      options:{},
      flights:{}
    },
      pageIndex: 1, // 当前的页码
      pageSize: 5, // 当前的条数
      total: 0 // 总条数
    };
  },
  components: {
    flightsListHead,
    flightsItem,
    FlightsFilters
  },
  mounted() {
    this.$axios({
      url: "airs",
      params: this.$route.query
    }).then(res => {
      console.log(res);
      this.airlist = res.data.flights;
      this.total = this.airlist.length;
        this.cacheFlightsData={...res.data}
      this.dataList = this.airlist.slice(0, this.pageSize);
      console.log(this.cacheFlightsData)
      
    });
  },
  methods: {
    handleSizeChange(val) {
      console.log(val);
      this.pageSize = val;
      this.dataList = this.airlist.slice(0, this.pageSize);
    },
    handleCurrentChange(val) {
      console.log(val);
      this.pageIndex = val;
      this.dataList = this.airlist.slice(
        (this.pageIndex - 1) * this.pageSize,
        this.pageIndex * this.pageSize
      );
    }
  }
};
</script>
<style scoped lang="less">
    .contianer{
        width:1000px;
        margin:20px auto;
    }

    .flights-content{
        width:745px;
        font-size:14px;
    }

    .aside{
        width:240px;
    } 
</style>