<template>
    <section class="echart-out-body">
        <div class="gate-scores-box" v-if="scoreshareNum">
            <div class="title">消耗积分中位数：</div>
            <div>{{scoreshareNum}}分</div>
        </div>
        <echart-tar v-if="echartData.title"
                    :id-name="'gateusers'"
                    :echartsDate="echartData"
                    ref="gateusers"></echart-tar>

        <div v-if="!echartData.title && isLoad" class="null-page">
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
            scoreshareNum: ''
        }
    },
    mounted () {
        this.getList()
        this.getData()
    },
    methods: {
        getList () {
            var formData = {
                interactionPrimeObject: this.$route.query.eventCode,
                interactionSubObject: this.$route.query.gameCode,
                interactionOtherObject: this.$route.query.gameGateCode
            }

            util.request({
                method: 'get',
                interface: 'gateusers',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.isLoad = true
                if (res.result.result) {
                    res.result.result.title = '关卡相关统计'
                    this.echartData = res.result.result
                    setTimeout(() => {
                        this.$refs['gateusers'].setEcharts()
                    }, 0)
                }
            })
        },
        getData () {
            var formData = {
                gameCode: this.$route.query.gameCode,
                gameGateCode: this.$route.query.gameGateCode
            }

            util.request({
                method: 'get',
                interface: 'gamepointconsume',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.scoreshareNum = res.result.result
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

    .gate-scores-box {
        display: flex;
        font-size: 17px;
        color: #333333;
        font-weight: bold;
        line-height: 32px;
        padding: 0 5px;
    }

    .echart-item {
        width: 100%;
        margin-bottom: 30px;
    }
}
</style>