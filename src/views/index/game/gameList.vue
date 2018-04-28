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
                    <div class="card-title" v-html="item.eventPlanTitle"></div>
                    <div class="card-desc" v-html="item.eventPlanDesc"></div>
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

                    <el-button v-if="item.eventStatus == '3'" type="info" :plain="true" size="small"
                        @click.prevent="downloadLog(index)">导出</el-button>
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

        <el-dialog title="领券记录导出" :visible.sync="isAddItem">
          <el-form :label-position="'left'" :model="addItemForm" label-width="80px">
            <el-form-item label="添加类型">
                <el-date-picker
                  v-model="addItemForm.dateRange"
                  type="daterange"
                  placeholder="选择日期范围">
                </el-date-picker>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
                <el-button @click="isAddItem = false">取 消</el-button>
                <el-button type="primary" @click="confirmItem">确定</el-button>
          </div>
        </el-dialog>
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
            total: 0,
            isAddItem: false,
            nowItemIndex: '',
            addItemForm: {
              dateRange: []
            }
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
        downloadLog (index) {
            this.isAddItem = true
            this.addItemForm = {
              dateRange: []
            }
            this.nowItemIndex = index
        },
        confirmItem () {
            var formData = {
                eventCode: this.marketList[this.nowItemIndex].eventCode,
                enterpriseCode: this.$route.query.enterpriseCode,
                startTime: '',
                endTime: ''
            }

            if (!this.addItemForm.dateRange[0] || !this.addItemForm.dateRange[1]) {
                this.$message({
                  type: 'warning',
                  message: '请选择日期范围!'
                })

                return
            }

            formData.startTime = util.formDataDate(this.addItemForm.dateRange[0]) + ' 00:00:00'
            formData.endTime = util.formDataDate(this.addItemForm.dateRange[1]) + ' 23:59:59'

            util.formRequest({
              method: 'get',
              interface: 'exportCouponLog',
              idName: 'exportCouponLog',
              data: formData
            })
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