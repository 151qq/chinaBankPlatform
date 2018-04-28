<template>
    <section class="echart-out-body">
        <echart-tar :id-name="'articlereading'" :echartsDate="echartData" ref="articlereading"></echart-tar>
    </section>
</template>
<script>
import util from '../../../assets/common/util'
import echartTar from '../../../components/common/echart-tar.vue'

export default {
    data () {
        return {
            isLoad: false,
            echartData: {}
        }
    },
    mounted () {
        this.getList()
    },
    methods: {
        getList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                interactionPrimeObject : this.$route.query.pageCode,
                interactionBeginDate: this.$route.query.pageCode.beginDate,
                interactionEndDate: this.$route.query.pageCode.endDate
            }

            util.request({
                method: 'get',
                interface: 'articlereading',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                res.result.result.title = '文章统计'

                this.echartData = res.result.result
                setTimeout(() => {
                    this.$refs['articlereading'].setEcharts()
                }, 0)
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
}
</style>