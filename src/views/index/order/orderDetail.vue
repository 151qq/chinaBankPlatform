<template>
    <div class="form-discount">
        <el-collapse v-model="activeNames" @change="collChange">
            <el-collapse-item class="float-form-box" title="订单信息" name="0">
                <order-base></order-base>
            </el-collapse-item>

            <div class="line-bold"></div>
            <el-collapse-item class="float-form-box" title="订单礼品" name="1">
                <gift-list></gift-list>
            </el-collapse-item>
        </el-collapse>          
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import orderBase from './formAlist/orderBase'
import giftList from './formAlist/giftList'

export default {
    data () {
        return {
            activeNames: ['0']
        }
    },
    mounted () {
        var orderColl = localStorage.getItem("orderColl")
        if (orderColl) {
            this.activeNames = orderColl.split(',')
        }
    },
    methods: {
        collChange () {
            localStorage.setItem("orderColl", this.activeNames)
        }
    },
    components: {
        orderBase,
        giftList
    }
}
</script>