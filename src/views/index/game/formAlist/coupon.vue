<template>
    <section class="discount-list-box">
        <router-link target="_blank"
                      v-if="base.eventStatus == '1' || base.eventStatus == '2'"
                      class="add-btn"
                            :to="{
                                name: 'coupon',
                                query: {
                                  enterpriseCode: $route.query.enterpriseCode,
                                  eventCode: $route.query.eventCode,
                                  eventType: '2'
                                }
                            }">
          <el-button class="add-btn" type="primary"
                    icon="plus" size="small">
            增加
          </el-button>
        </router-link>
        <el-table
          :data="couponData"
          border
          style="width: 100%">
          <el-table-column
            prop="couponTitle"
            label="优惠券名称"
            width="300">
          </el-table-column>
          <el-table-column
            prop="couponTypeName"
            label="优惠券类型">
          </el-table-column>
          <el-table-column
            prop="couponViewName"
            label="说明">
          </el-table-column>
          <el-table-column
            v-if="isEdit"
            label="操作"
            width="100">
            <template scope="scope">
              <i class="btn-box el-icon-delete2"
                  v-if="base.eventStatus == '1' || base.eventStatus == '2'"
                  @click="deleteCoupon(scope.row.couponCode)"></i>
  
              <router-link  class="btn-box el-icon-document"
                            target="_blank"
                            :to="{
                              name: 'coupon',
                              query: {
                                enterpriseCode: $route.query.enterpriseCode,
                                eventCode: $route.query.eventCode,
                                couponCode: scope.row.couponCode,
                                eventType: '2'
                              }
                            }">
              </router-link>
            </template>
          </el-table-column>
        </el-table>
        
        <el-pagination
            v-if="total"
            class="page-box"
            @current-change="pageChange"
            layout="prev, pager, next"
            :page-size="pageSize"
            :total="total">
        </el-pagination>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import popupImg from '../../../../components/common/popupImg.vue'
import popupLoad from '../../../../components/common/popupLoad.vue'
import { mapGetters } from 'vuex'

export default {
    props: ['base'],
    data () {
        return {
          couponData: [],
          pageNumber: 1,
          pageSize: 1000,
          total: 0
        }
    },
    mounted () {
      this.getList()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          return this.$route.query.enterpriseCode == this.userInfo.enterpriseCode
        }
    },
    methods: {
      pageChange (size) {
        this.pageNumber = size
        this.getList()
      },
      getList () {
        util.request({
            method: 'get',
            interface: 'couponInfoList',
            data: {
              enterpriseCode: this.$route.query.enterpriseCode,
              eventCode: this.$route.query.eventCode,
              pageNumber: this.pageNumber,
              pageSize: this.pageSize
            }
        }).then(res => {
            if (res.result.success == '1') {
                if (!res.result.result.length) {
                  return false
                }

                this.total = Number(res.result.total)
                
                this.couponData = res.result.result

                this.$emit('hasCoupon', res.result.result)
            } else {
                this.$message.error(res.result.message)
            }
        })
      },
      addQuan () {
          var pathData = {
            name: 'coupon',
            query: {
              enterpriseCode: this.$route.query.enterpriseCode,
              eventCode: this.$route.query.eventCode,
              eventType: '2'
            }
          }

          this.$router.push(pathData)
      },
      deleteCoupon (code) {
        util.request({
            method: 'get',
            interface: 'couponInfoDelete',
            data: {
              couponCode: code,
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
      }
    }
}
</script>
<style lang="scss">
.discount-list-box {
  .page-box {
    margin-top: 20px;
  }

  .btn-box {
    color: #000000;
    cursor: pointer;
  }
}
</style>