<template>
    <div class="market-list-big-box">
        <el-tabs v-model="activeName">
            <el-tab-pane label="吸粉活动" name="1">
                <game-list></game-list>
            </el-tab-pane>

            <el-tab-pane label="促销活动" name="2">
                <market-list></market-list>
            </el-tab-pane>
        </el-tabs>
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import marketList from './marketList'
import gameList from '../game/gameList'

export default {
    data () {
        return {
            activeName: '1'
        }
    },
    components: {
      marketList,
      gameList
    }
}
</script>
<style lang="scss">
.market-list-big-box {
  width: 1000px;
  margin: 80px auto 30px;

  .el-tabs__content {
    overflow: visible;
  }
}
</style>