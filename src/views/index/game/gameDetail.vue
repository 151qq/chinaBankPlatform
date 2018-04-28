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

            <template v-if="baseData.eventCode">
                <div class="line-bold"></div>
                <el-collapse-item class="float-form-box" title="游戏基本配置" name="3">
                    <game-base :base="baseData" @hasGame="hasGame"></game-base>
                </el-collapse-item>

                <template v-if="gameDetail.gameCode">
                    <div class="line-bold"></div>
                    <el-collapse-item class="float-form-box" title="游戏关卡配置" name="4">
                        <barrier-list :base="baseData" :coupon-list="couponList"
                                        :game-code="gameDetail.gameCode"></barrier-list>
                    </el-collapse-item>
                </template>

                <div class="line-bold"></div>
                <el-collapse-item class="float-form-box" title="活动推广文章" name="2">
                    <form-article :base="baseData"></form-article>
                </el-collapse-item>
            </template>
        </el-collapse>    
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import marketBase from './formAlist/marketBase'
import gameBase from './formAlist/gameBase'
import coupon from './formAlist/coupon'
import formArticle from './formAlist/formArticle'
import barrierList from './formAlist/barrierList'

export default {
    data () {
        return {
            isPage: false,
            baseData: {},
            activeNames: ['0'],
            couponList: [],
            gameDetail: {}
        }
    },
    mounted () {
        var gameColl = localStorage.getItem("gameColl")
        if (gameColl) {
            this.activeNames = gameColl.split(',')
        }
    },
    methods: {
        hasBase (base) {
            this.baseData = Object.assign({}, base)
        },
        hasCoupon (datas) {
            this.couponList = [].concat(datas)
        },
        hasGame (base) {
            this.gameDetail = Object.assign({}, base)
        },
        collChange () {
            localStorage.setItem("gameColl", this.activeNames)
        }
    },
    components: {
        marketBase,
        barrierList,
        coupon,
        formArticle,
        gameBase
    }
}
</script>