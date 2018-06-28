<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox">
            <span>下单人：</span>
            <div class="input-box">
              {{base.memberName}}
            </div>
        </section>
        <section class="formBox">
            <span>收货人：</span>
            <div class="input-box">
              {{base.orderReceiver}}
            </div>
        </section>
        <section class="formBox">
            <span>收货电话：</span>
            <div class="input-box">
              {{base.orderReceiverMobile}}
            </div>
        </section>
        <section class="formBox">
            <span>下单时间：</span>
            <div class="input-box">
              {{base.orderTime.split(' ')[0]}}
            </div>
        </section>
        <section class="formBox">
            <span>省：</span>
            <div class="input-box">
              {{base.orderAddrProvince}}
            </div>
        </section>
        <section class="formBox">
            <span>市：</span>
            <div class="input-box">
              {{base.orderAddrCity}}
            </div>
        </section>
        <section class="formBox bigF">
            <span>详细地址：</span>
            <div class="input-box">
              {{base.orderAddr}}
            </div>
        </section>
        <section class="formBox">
            <span>物流编号</span>
            <el-input
              class="input-box"
              placeholder="请输入内容"
              v-model="base.orderLogisticNumber">
            </el-input>
        </section>
        <section class="formBox">
            <span>物流公司</span>
            <el-input
              class="input-box"
              placeholder="请输入内容"
              v-model="base.orderLogisticUnit">
            </el-input>
        </section>
        <section class="formBox">
            <span>快递时间</span>
            <el-date-picker
              class="input-box"
              type="date"
              v-model="base.orderLogisticDate"
              placeholder="选择">
            </el-date-picker>
        </section>
      </div>
      <el-button v-if="isEdit" class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase">提交</el-button>
      <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    data () {
        return {
            base: {
              memberName: '',
              orderReceiver: '',
              orderReceiverMobile: '',
              orderTime: '',
              orderAddrProvince: '',
              orderAddrCity: '',
              orderAddr: '',
              orderStatus: '',
              orderLogisticNumber: '',
              orderLogisticUnit: '',
              orderLogisticDate: ''
            }
        }
    },
    mounted () {
      this.getBase()
    },
    computed: {
      ...mapGetters({
          userInfo: 'getUserInfo'
      }),
      isEdit () {
        return this.base.orderStatus == '1'
      }
    },
    methods: {
        getBase () {
          util.request({
              method: 'post',
              interface: 'organizeOrderInfo',
              data: {
                orderCode: this.$route.query.orderCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.base = res.result.result
                this.$emit('hasBase', res.result.result)
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        saveBase () {
            if (!this.base.orderLogisticNumber) {
                this.$message({
                    message: '请填写物流编号！',
                    type: 'warning'
                })
                return false
            }

            if (!this.base.orderLogisticUnit) {
                this.$message({
                    message: '请填写物流公司！',
                    type: 'warning'
                })
                return false
            }

            if (!this.base.orderLogisticDate) {
                this.$message({
                    message: '请填写快递时间！',
                    type: 'warning'
                })
                return false
            }

            this.base.enterpriseCode = this.$route.query.enterpriseCode
            this.base.orderTime = new Date(this.base.orderTime)
            this.base.orderLogisticDate = new Date(this.base.orderLogisticDate)
            this.base.orderEnterpriseUser = this.userInfo.userCode
            this.base.orderStatus = '3'

            util.request({
                method: 'post',
                interface: 'organizeOrderSave',
                data: this.base
            }).then(res => {
                if (res.result.success == '1') {
                    this.$message({
                      type: 'success',
                      message: '提交成功!'
                    })

                    this.getBase()
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
    }
}
</script>