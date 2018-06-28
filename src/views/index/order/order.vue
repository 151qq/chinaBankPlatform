<template>
    <div class="order-list-big-box">
        <el-tabs v-model="activeName">
            <el-tab-pane v-for="(item, index) in tabDatas"
                          :label="item.label" :name="item.value">
                <order-list :order-type="item.value"></order-list>
            </el-tab-pane>
        </el-tabs>
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import orderList from './orderList'

export default {
    data () {
        return {
            tabDatas: [
              {
                label: '待处理',
                value: '1'
              },
              {
                label: '已处理',
                value: '3'
              }
            ],
            activeName: '1'
        }
    },
    components: {
      orderList
    }
}
</script>
<style lang="scss">
.order-list-big-box {
  width: 1000px;
  margin: 80px auto 30px;

  .el-tabs__content {
    overflow: visible;
  }
}
</style>