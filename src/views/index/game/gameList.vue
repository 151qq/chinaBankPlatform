<template>
    <div class="market-list-box duan-list-box">
        <div class="btn-input-box">
            <router-link target="_blank"
                            class="add-new-btn"
                            :to="{
                                name: 'game-detail',
                                query: {
                                    enterpriseCode: this.$route.query.enterpriseCode
                                }
                            }">
                <el-button  class="add-new-btn"
                            type="primary"
                           icon="plus">
                    增加
                </el-button>
            </router-link>
        </div>
        <section class="big-cards-box">
            <router-link class="card-box"
                         target="_blank"
                         v-for="(item, index) in marketList"
                         :to="{name: 'game-detail', query: {eventCode: item.eventCode, enterpriseCode: item.enterpriseCode}}">
                <div class="card-img">
                    <img v-if="item.eventPlanCover" :src="item.eventPlanCover">
                </div>
                
                <div class="card-content">
                    <div class="card-title">{{item.eventPlanTitle}}</div>
                    <div class="card-desc">{{item.eventPlanDesc}}</div>
                    <div class="card-tag">
                        <el-tag v-if="item.eventStatus == '1' || item.eventStatus == '2'" type="gray">草稿</el-tag>
                        <el-tag v-if="item.eventStatus == '3'" type="success">已发布</el-tag>
                        <el-tag v-if="item.eventStatus == '5'">正常结束</el-tag>
                        <el-tag v-if="item.eventStatus == '4'">提前终止</el-tag>
                    </div>
                </div>
                <section class="card-btns">
                    <el-button v-if="item.eventStatus == '1' || item.eventStatus == '2'" type="danger" :plain="true" size="small"
                        @click.prevent="deleteItemByCode(item)">删除</el-button>

                    <el-button v-if="item.eventStatus == '1' || item.eventStatus == '2'" type="info" :plain="true" size="small"
                        @click.prevent="submitItemByCode(item)">发布</el-button>

                    <el-button v-if="item.eventStatus == '3'" type="info" :plain="true" size="small"
                        @click.prevent="stopItemByCode(item)">终止</el-button>
                </section>
            </router-link>
        </section>
        <section class="null-box" v-if="!marketList.length && isPage">
          暂无内容！！！
        </section>
        <el-pagination
            v-if="total"
            class="page-box"
            @current-change="pageChange"
            layout="prev, pager, next"
            :page-size="pageSize"
            :total="total">
        </el-pagination>
  </div>
</template>
<script>
import util from '../../../assets/common/util'
import { mapGetters } from 'vuex'

export default {
    data () {
        return {
            isPage: false,
            keyValue: '',
            marketList: [],
            pageSize: 20,
            pageNumber: 1,
            total: 0
        }
    },
    mounted () {
        this.getList()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        })
    },
    methods: {
        searchItem () {
          this.getList()
        },
        getList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                eventDesigner: this.userInfo.userCode,
                eventType: '2',
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.keyValue) {
                formData.keyValue = this.keyValue
            }

            util.request({
                method: 'post',
                interface: 'eventInfoList',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.total = Number(res.result.total)
                this.isPage = true
                this.marketList = res.result.result
            })
        },
        deleteItemByCode (item) {
            util.request({
              method: 'post',
              interface: 'eventInfoDelete',
              data: {
                eventCode: item.eventCode,
                eventType: '2'
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()

                this.$message({
                  type: 'success',
                  message: '删除成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
            })
        },
        submitItemByCode (item) {
            util.request({
              method: 'post',
              interface: 'eventGameCreateCoupon',
              data: {
                eventCode: item.eventCode,
                enterpriseCode: this.$route.query.enterpriseCode
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()

                this.$message({
                  type: 'success',
                  message: '发布成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
            })
        },
        stopItemByCode (item) {
            util.request({
              method: 'post',
              interface: 'stopEvent',
              data: {
                eventCode: item.eventCode,
                enterpriseCode: this.$route.query.enterpriseCode
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()

                this.$message({
                  type: 'success',
                  message: '终止成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
            })
        },
        addItem () {
            var pathUrl = {
              name: 'game-detail',
              query: {
                enterpriseCode: this.$route.query.enterpriseCode
              }
            }

            this.$router.push(pathUrl)
        },
        pageChange (size) {
            this.pageNumber = size
            this.getList()
        }
    }
}
</script>
<style lang="scss">
.market-list-box {
    margin: auto;
}
</style>