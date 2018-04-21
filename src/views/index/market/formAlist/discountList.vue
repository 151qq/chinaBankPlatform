<template>
    <section class="product-base-box">
        <div class="formDiscount">
          <section class="formBox bigF">
              <span>投放优惠券</span>
              <el-select
                class="input-box"
                v-model="couponCodes"
                multiple
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in couponList"
                  :key="index"
                  :label="item.couponTitle"
                  :value="item.couponCode">
                </el-option>
              </el-select>
          </section>
          <section class="formBox">
            <span>券架封面</span>
            <div class="input-box">
              <upload :path="quanData.couponGroupCover"
                      :bg-path="false"
                      :is-operate="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')"
                      @changeImg="changeImg"></upload>
            </div>
          </section>
          <section class="formBox">
              <span>券架标题</span>
              <el-input
                class="input-box"
                placeholder="请输入内容,最多9个字"
                :maxlength="9"
                v-model="quanData.couponGroupName">
              </el-input>
          </section>
          <section class="formBox">
              <span>广告用语</span>
              <el-input
                class="input-box"
                placeholder="请输入内容,最多15个字"
                :maxlength="15"
                v-model="quanData.couponGroupIntro">
              </el-input>
          </section>
        </div>
        <el-button v-if="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')" class="save-btn" type="info" :plain="true" size="small" icon="document"
            @click="confirmQuan">保存</el-button>
        <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import upload from '../../../../components/common/upload'
import { mapGetters } from 'vuex'
export default {
    props: ['base', 'groupList', 'couponScenario', 'couponList'],
    data () {
        return {
            quanData: {
              eventCode: '',
              enterpriseCode: '',
              couponGroupName: '',
              couponGroupBeginTimestamp: '',
              couponGroupEndTimestamp: '',
              couponGroutScenario: '',
              couponGroupCover: '',
              couponGroupIntro: '',
              couponCode: ''
            },
            couponCodes: []
        }
    },
    mounted () {
      setTimeout(() => {
        this.setGroup()
      }, 30)
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
      setGroup () {
        if (!this.groupList.length) {
          return false
        }

        this.groupList.forEach((item) => {
          if (item.couponGroutScenario == this.couponScenario) {
            this.couponCodes = item.couponCode.split(',')
            this.quanData = Object.assign({}, item)
          }
        })
      },
      changeImg (data) {
        this.quanData.couponGroupCover = data.url
      },
      confirmQuan () {
        if (!this.quanData.couponGroupName) {
          this.$message({
              message: '请填写券架标题！',
              type: 'warning'
          })
          return false
        }

        if (!this.quanData.couponGroupCover) {
          this.$message({
              message: '请选择券架封面！',
              type: 'warning'
          })
          return false
        }

        if (!this.quanData.couponGroupIntro) {
          this.$message({
              message: '请填写广告用语！',
              type: 'warning'
          })
          return false
        }

        if (!this.couponCodes.length) {
          this.$message({
              message: '请选择投放券！',
              type: 'warning'
          })
          return false
        }

        this.quanData.couponCode = this.couponCodes.join(',')
        this.quanData.couponGroutScenario = this.couponScenario
        this.quanData.enterpriseCode = this.$route.query.enterpriseCode
        this.quanData.eventCode = this.$route.query.eventCode

        if (!this.quanData.couponGroupBeginTimestamp || !this.quanData.couponGroupEndTimestamp) {
          var eventEndTime = Math.floor(new Date(this.base.eventEndTime).getTime()/1000)
          var eventStartTime = Math.floor(new Date(this.base.eventStartTime).getTime()/1000)

          this.quanData.couponGroupBeginTimestamp = eventStartTime
          this.quanData.couponGroupEndTimestamp = eventEndTime
        }

        this.insertQuan()
      },
      insertQuan () {
        util.request({
            method: 'post',
            interface: 'manageCouponGroup',
            data: this.quanData
        }).then((res) => {
            if (res.result.success == '0') {
              this.$message.error(res.result.message)
            } else {
              this.$message({
                type: 'success',
                message: '恭喜你，保存成功!'
              })
            }
        })
      }
    },
    components: {
      upload
    }
}
</script>
<style lang="scss">
.product-base-box {
  overflow: hidden;

  .form-box {
    overflow: hidden;
  }
}
</style>