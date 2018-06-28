<template>
    <div>
        <el-table
          :data="marketList"
          border
          style="width: 100%">
          <el-table-column
            prop="ownerStockRight"
            label="订单时间">
            <template scope="scope">
                {{scope.row.orderTime.split(' ')[0]}}
            </template>
          </el-table-column>
          <el-table-column
            prop="memberName"
            label="下单人">
          </el-table-column>
          <el-table-column
            prop="memberMobile"
            label="下单人手机号">
          </el-table-column>
          <el-table-column
            label="操作"
            width="70">
            <template scope="scope">
                <router-link class="el-icon-document colorBlack"
                         target="_blank"
                         :to="{
                            name: 'order-detail',
                            query: {
                              enterpriseCode: scope.row.enterpriseCode,
                              orderCode: scope.row.orderCode
                          }}">
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
  </div>
</template>
<script>
import util from '../../../assets/common/util'
import { mapGetters } from 'vuex'

export default {
    props: ['orderType'],
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
                orderStatus: this.orderType,
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.keyValue) {
                formData.subjectContent = this.keyValue
            }

            util.request({
                method: 'post',
                interface: 'organizeOrderList',
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
        pageChange (size) {
            this.pageNumber = size
            this.getList()
        }
    }
}
</script>