<template>
  <div class="wrap">
    <div class="background"></div>
    <div class="top-line"></div>
    <main>
      <search class="search"></search>
      <h2>
        {{$t('blocks.title')}}
      </h2>
      <div class="total">
        A Total Of {{total}} blocks found
      </div>
      <div class="table-wrap">
        <el-table
          :data="blocks"
          style="width: 100%"
        >
          <el-table-column
            align="center"
            :label="$t('blocks.height')"
          >
            <template slot-scope="scope">
              <router-link :to="'/blockDetails/'+scope.row.blockNum">{{scope.row.blockNum}}</router-link>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            :label="$t('blocks.age')"
            width="180">
            <template slot-scope="scope">
              <timeago :since="scope.row.blockTime" :locale="getBusLocal" :auto-update="0.5"></timeago>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            prop="trxCount"
            :label="$t('blocks.txn')"
          >
          </el-table-column>
          <el-table-column
            align="center"
            :label="$t('blocks.miner')"
            show-overflow-tooltip
          >
            <template slot-scope="scope">
              <router-link :to="'/address?minerName='+scope.row.minerName">{{scope.row.minerName}}</router-link>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            prop="rewards"
            :label="$t('blocks.reward')"
            width="180"
          >
          </el-table-column>
        </el-table>
      </div>
      <el-pagination
        class="pagination"
        layout="prev, pager, next, jumper"
        :current-page="page"
        :page-size="size"
        :total="total"
      @current-change="pageChange">
      </el-pagination>
    </main>
  </div>
</template>

<script>
  import Search from "../components/search/Search";
  import bus from "../utils/bus";

  export default {
    components: {Search},
    name: "blocks",
    data() {
      return {
        blocks: [],
        page: 1,
        size: 25,
        total: 0
      };
    },
    created() {
      this.getBlocksList();
    },
    methods: {
      getBlocksList() {
        let that = this;
        this.$axios.post('/queryBlockList',{page:this.page,rows:this.size}).then(function (res) {
          let data = res.data.data;
          that.blocks = data.rows;
          that.total = data.total;
        })
      },
      pageChange(page) {
        this.page = page;
        this.getBlocksList();
      }
    },
    computed: {
      getBusLocal() {
        return bus.local;
      }
    }
  }
</script>

<style lang="less" scoped>
  .wrap {
    .top-line {
      height: 1px;
    }
    .background {
      width: 100%;
      height: 338px;
      position: absolute;
      top: 0;
      left: 0;
      background: url("../assets/img/home-bg.png");
    }
    main {
      width: 77%;
      min-width: 1160px;
      margin: 120px auto;
      position: relative;
      color: white;
      .search {
        position: absolute;
        right: 0;
        top: 0;
        /*transform: translateY(-50%);*/
      }
      h2 {
        font-size: 36px;
      }
      .total {
        margin: 10px 0 65px;
      }
      .pagination {
        margin-top: 20px;
        text-align: center;
      }
    }
  }
</style>
