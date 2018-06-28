<template>
    <section class="order-gift-box">
        <template v-if="sourceDatas.length">
            <section class="check-box" v-for="(item, index) in sourceDatas" :key="index">
                <router-link class="sou-box"
                            target="_blank"
                            :to="{
                                name: 'gift-list',
                                query: {
                                    enterpriseCode: $route.query.enterpriseCode,
                                    productCode: item.productCode
                                }
                            }">
                    <div class="cover-box">
                        <img :src="item.productCover">
                    </div>
                    <div class="title-box">
                        <div class="title">{{item.productCname}}</div>
                    </div>
                </router-link>
            </section>
            
            <div class="clear"></div>
        </template>
        <!-- 无内容 -->
        <section v-if="isPage && !sourceDatas.length" class="null-page">
            暂无礼品！！！
        </section>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'

export default {
    data () {
        return {
            isPage: false,
            sourceDatas: []
        }
    },
    mounted () {
        this.getList()
    },
    methods: {
        getList () {
            var formData = {
                orderCode: this.$route.query.orderCode
            }

            util.request({
                method: 'get',
                interface: 'organizeOrderProductInfo',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.isPage = true
                this.sourceDatas = res.result.result
            })
        }
    }
}
</script>
<style lang="scss">
.order-gift-box {
    .check-box {
        position: relative;
        float: left;
        width: 170px;
        margin-right: 20px;
        margin-bottom: 20px;
    }

    .sou-box {
        border: 1px solid #D3DCE6;
        border-radius: 3px;
        display: block;

        .cover-box {
            display: block;
            height: 130px;
            overflow: hidden;
            cursor: pointer;
            background: #f1f1f1;
            
            img {
                display: block;
                width: 100%;
                height: 130px;
            }
        }

        .title-box {
            padding: 5px 10px;
            border-top: 1px solid #D3DCE6;
            
            .title {
                display: block;
                font-size: 14px;
                line-height: 20px;
                border: none;
                color: #000000;
            }

            .time {
                display: block;
                font-size: 14px;
                height: 20px;
                line-height: 20px;
                color: #999999;
            }

            .btn-box {
               float: right;
               font-size: 14px;
               color: #333333;
               margin-left: 5px;

               i, label {
                    cursor: pointer;
               }
            }
        }
    }
}
</style>