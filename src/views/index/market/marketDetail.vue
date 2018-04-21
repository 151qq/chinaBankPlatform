<template>
    <div class="form-discount">
        <el-collapse v-model="activeNames" @change="collChange">
            <el-collapse-item class="float-form-box" title="基本信息" name="0">
                <market-base @hasBase="hasBase"></market-base>
            </el-collapse-item>

            <template v-if="baseData.eventCode">
                <div class="line-bold"></div>
                <el-collapse-item class="float-form-box" title="优惠券列表" name="1">
                    <coupon :base="baseData" @hasCoupon="hasCoupon"></coupon>
                </el-collapse-item>
            </template>

            <template v-for="(item, index) in discountData" v-if="baseData.eventCode && isPage && couponList.length">
                <div class="line-bold"></div>
                <el-collapse-item class="float-form-box"
                                    :title="item.typeName + '设置'" :name="index + 2">
                    <discount-list :base="baseData"
                                    :group-list="groupList"
                                    :coupon-list="couponList"
                                    :coupon-scenario="item.id"></discount-list>
                </el-collapse-item>
            </template>
        </el-collapse>          
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import marketBase from './formAlist/marketBase'
import coupon from './formAlist/coupon'
import discountList from './formAlist/discountList'

export default {
    data () {
        return {
            isPage: false,
            id: 0,
            baseData: {},
            activeNames: ['0'],
            discountData: [],
            groupList: [],
            couponList: []
        }
    },
    mounted () {
        var marketColl = localStorage.getItem("marketColl")
        if (marketColl) {
            this.activeNames = marketColl.split(',')
        }
    },
    methods: {
        hasBase (base) {
            this.baseData = Object.assign({}, base)

            if (this.baseData.eventCode) {
                this.getTypes()
            }
        },
        hasCoupon (datas) {
            this.couponList = [].concat(datas)
            if (this.couponList.length) {
                this.getList()
            }
        },
        collChange () {
            localStorage.setItem("marketColl", this.activeNames)
        },
        getList () {
            util.request({
                method: 'get',
                interface: 'couponGroupOfPage',
                data: {
                  enterpriseCode: this.$route.query.enterpriseCode,
                  eventCode: this.$route.query.eventCode,
                  pageNumber: this.pageNumber,
                  pageSize: this.pageSize
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.isPage = true
                    
                    if (!res.result.result.length) {
                      return false
                    }

                    this.total = Number(res.result.total)
                    this.groupList = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getTypes () {
            util.request({
                method: 'get',
                interface: 'disOfEnterpriseInfo',
                data: {
                    types: 'coupon_scenario'
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.discountData = res.result.result.coupon_scenario
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
    },
    components: {
        marketBase,
        discountList,
        coupon
    }
}
</script>