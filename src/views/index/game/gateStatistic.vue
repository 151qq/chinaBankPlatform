<template>
    <section class="echart-out-body">
        <div class="echart-item">
            <echart-tar :id-name="'gateusers'" :echartsDate="echartData" ref="gateusers"></echart-tar>
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

                if (res.result.result) {
                    res.result.result.title = '关卡相关统计'
                }
                
                this.echartData = res.result.result
                setTimeout(() => {
                    this.$refs['gateusers'].setEcharts()
                }, 0)
            })
        },
        getData () {
            var formData = {
                interactionPrimeObject: this.$route.query.eventCode,
                interactionSubObject: this.$route.query.gameCode,
                interactionOtherObject: this.$route.query.gameGateCode
            }

            util.request({
                method: 'get',
                interface: 'gamescoreshare',
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

    .echart-item {
        width: 100%;
        margin-bottom: 30px;
    }
}
</style>