<template>
    <section class="echart-out-body">
        <div class="echart-item" v-if="echartData.title">
            <echart-tar :id-name="'followusers'" :echartsDate="echartData" ref="followusers"></echart-tar>
        </div>
        <div class="echart-item" v-if="echartDataUser.title">
            <echart-tar :id-name="'gameusers'" :echartsDate="echartDataUser" ref="gameusers"></echart-tar>
        </div>
        <div class="echart-item" v-if="echartDataWinner.title">
            <echart-tar :id-name="'gatewinners'" :echartsDate="echartDataWinner" ref="gatewinners"></echart-tar>
        </div>

        <div v-if="!echartData.title && !echartDataUser.title && isLoad" class="null-page">
            暂无统计
        </div>
    </section>
</template>
<script>
import util from '../../../assets/common/util'
import echartTar from '../../../components/common/echart-tar.vue'

export default {
    data () {
        return {
            isLoad: false,
            echartData: {},
            echartDataUser: {},
            echartDataWinner: {}
        }
    },
    mounted () {
        this.getList()
        this.getData()
        this.getWinners()
    },
    methods: {
        getList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                eventCode: this.$route.query.eventCode,
                interactionBeginDate: this.$route.query.beginDate,
                interactionEndDate: this.$route.query.endDate
            }

            util.request({
                method: 'get',
                interface: 'followusers',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.isLoad = true
                if (res.result.result) {
                    res.result.result.title = '活动关注数统计'
                    this.echartData = res.result.result
                    setTimeout(() => {
                        this.$refs['followusers'].setEcharts()
                    }, 0)
                }
            })
        },
        getData () {
            var formData = {
                interactionPrimeObject: this.$route.query.eventCode
            }

            util.request({
                method: 'get',
                interface: 'gameusers',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.isLoad = true
                if (res.result.result) {
                    res.result.result.title = '玩游戏人数统计'
                    this.echartDataUser = res.result.result
                    setTimeout(() => {
                        this.$refs['gameusers'].setEcharts()
                    }, 0)
                }
            })
        },
        getWinners () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                eventCode: this.$route.query.eventCode
            }

            util.request({
                method: 'get',
                interface: 'gatewinners',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.isLoad = true
                if (res.result.result) {
                    res.result.result.title = '中奖人数统计'
                    this.echartDataWinner = res.result.result
                    setTimeout(() => {
                        this.$refs['gatewinners'].setEcharts()
                    }, 0)
                }
            })
        }
    },
    components: {
        echartTar
    }
}
</script>
<style lang="scss">
.echart-out-body {
    width: 1000px;
    margin: 80px auto 30px;

    .echart-item {
        width: 100%;
        margin-bottom: 30px;
    }
}
</style>