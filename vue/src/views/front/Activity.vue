<template>
  <div class="main-content">

    <div style="display: flex; width: 70%; margin: 30px auto">
      <el-row :gutter="10" >
        <el-col :span="6" v-for="item in activityList" :key="item.id">
          <div style="margin-bottom: 10px; cursor: pointer;" @click="$router.push('/front/activityDetail?id=' + item.id)">
            <img :src="item.cover" alt="" style="width: 250px; height: 150px; display: block; border-radius: 5px 5px 0 0;">
            <div style="padding: 10px">
              <div class="line2" style="height: 40px; margin-bottom: 5px">{{ item.name }}</div>
              <div style="font-size: 13px;  color: #666;">{{ item.start }} ~ {{ item.end }}</div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>

    <div style="display: flex; margin: 10px 0; justify-content: center;" v-if="total">
      <el-pagination
          background
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-size="pageSize"
          layout="total, prev, pager, next"
          :total="total">
      </el-pagination>
    </div>

  </div>
</template>

<script>

export default {
  name: "FrontActivity",
  data() {
    return {
      activityList: [],
      pageNum: 1,
      pageSize: 8,
      total: 0
    }
  },
  created() {
    this.load(1)
  },
  methods: {
    load(pageNum) {  // 分页查询
      if (pageNum) this.pageNum = pageNum
      this.$request.get('/activity/selectPage', {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          name: this.name,
        }
      }).then(res => {
        this.activityList = res.data?.list
        this.total = res.data?.total
      })
    },
    handleCurrentChange(pageNum) {
      this.load(pageNum)
    },
  }
}
</script>

<style scoped>

</style>