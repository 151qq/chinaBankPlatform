<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox bigF">
            <span>套券名称</span>
            <el-input
              class="input-box"
              placeholder="请输入内容,最多16个字"
              :maxlength="16"
              v-model="base.eventPlanTitle">
            </el-input>
        </section>
        <section class="formBox">
            <span>生效时间</span>
            <el-date-picker
              class="input-box"
              type="date"
              :disabled="!!$route.query.eventCode"
              :picker-options="pickerOptions"
              v-model="base.eventStartTime"
              placeholder="选择">
            </el-date-picker>
        </section>
        <section class="formBox">
            <span>失效时间</span>
            <el-date-picker
              class="input-box"
              type="date"
              :disabled="!!$route.query.eventCode"
              :picker-options="pickerOptionsEnd"
              v-model="base.eventEndTime"
              placeholder="选择">
            </el-date-picker>
        </section>
        <section class="formBox">
            <span>活动类型</span>
            <el-select
              class="input-box"
              v-model="base.eventType"
              :disabled="true"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in eventTypes"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>新增潜客</span>
            <el-input-number class="input-box" size="small" :min="0"
                  v-model="base.eventLeads"></el-input-number>
        </section>
        <section class="formBox">
            <span>新增商机</span>
            <el-input-number class="input-box" size="small" :min="0"
                  v-model="base.eventHotLeads"></el-input-number>
        </section>
        <section class="formBox">
            <span>新增销售</span>
            <el-input-number class="input-box" size="small" :min="0"
                  v-model="base.eventSalesOpp"></el-input-number>
        </section>
        <section class="formBox bigF">
            <span>套券描述</span>
            <el-input
              type="textarea"
              :rows="4"
              :maxlength="140"
              placeholder="请输入内容,最多140字"
              v-model="base.eventPlanDesc">
            </el-input>
            <div class="limit-box">剩余<a>{{descNum}}</a>字</div>
        </section>
        <section class="formBox">
          <span>标准照片</span>
          <div class="input-box">
            <upload :path="base.eventPlanCover"
                    :bg-path="false"
                    :is-operate="isEdit"
                    @changeImg="changeImg"></upload>
          </div>
        </section>
      </div>
      <el-button v-if="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')" class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase()">保存</el-button>
      <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import upload from '../../../../components/common/upload'
import { mapGetters } from 'vuex'
export default {
    data () {
        return {
            base: {
              eventPlanTitle: '',
              eventStartTime: '',
              eventEndTime: '',
              eventStatus: '1',
              eventLeads: '',
              eventType: '1',
              eventHotLeads: '',
              eventSalesOpp: '',
              eventPlanDesc: '',
              eventPlanCover: ''
            },
            statusTypes: [],
            isOperate: true,
            pickerOptions: {
              disabledDate(time) {
                return time.getTime() < Date.now() + 3600 * 1000 * 24 * 5
              }
            },
            eventTypes: [
              {
                key: '促销',
                value: '1'
              },
              {
                key: '游戏营销',
                value: '2'
              }
            ]
        }
    },
    mounted () {
      if (this.$route.query.eventCode) {
        this.getBase()
      }
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          return this.$route.query.enterpriseCode == this.userInfo.enterpriseCode
        },
        descNum () {
          return 140 - this.base.eventPlanDesc.length
        },
        pickerOptionsEnd () {
          var eventStartTime = this.base.eventStartTime

          return {
            disabledDate(time) {
              var todayTime = eventStartTime ? new Date(eventStartTime).getTime() : Date.now() + 3600 * 1000 * 24 * 5
              var isLtoday = time.getTime() < todayTime
              var isRtoday = time.getTime() > todayTime + 3600 * 1000 * 24 * 31
              return isLtoday || isRtoday
            }
          }
        }
    },
    watch: {
      $route () {
        if (this.$route.query.eventCode) {
          this.getBase()
        }
      }
    },
    methods: {
        getBase () {
          util.request({
              method: 'get',
              interface: 'eventInfoGet',
              data: {
                eventCode: this.$route.query.eventCode
              }
          }).then(res => {
              this.base = res.result.result
              this.$emit('hasBase', res.result.result)
          })
        },
        changeImg (data) {
          this.base.eventPlanCover = data.url
        },
        formDataDate (str) {
            var dateStr = new Date(str)
            var year = dateStr.getFullYear()
            var monthStr = dateStr.getMonth() + 1
            var dayStr = dateStr.getDate()
            var month = monthStr < 10 ? '0' + monthStr : monthStr
            var day = dayStr < 10 ? '0' + dayStr : dayStr
            return year + '-' + month + '-' + day
        },
        saveBase () {
            if (this.base.eventPlanTitle.trim() == '') {
                this.$message({
                    message: '请填写套券名称！',
                    type: 'warning'
                })
                return false
            }

            if (!this.base.eventStartTime) {
              this.$message({
                    message: '请填写生效时间！',
                    type: 'warning'
                })
                return false
            }

            var startTimes = new Date(this.base.eventStartTime).getTime()

            if (new Date().getTime() > startTimes) {
              this.$message({
                  message: '生效时间必须大于当前时间！',
                  type: 'warning'
              })
              return false
            }

            if (!this.base.eventEndTime) {
              this.$message({
                    message: '请填写失效时间！',
                    type: 'warning'
                })
                return false
            }

            var endTimes = new Date(this.base.eventEndTime).getTime()

            if (endTimes <= startTimes) {
              this.$message({
                  message: '失效时间必须大于生效时间！',
                  type: 'warning'
              })
              return false
            }

            if (endTimes - startTimes > 31 * 24 * 3600 * 1000) {
              this.$message({
                  message: '活动周期最长31天！',
                  type: 'warning'
              })
              return false
            }

            this.base.eventStartTime = this.formDataDate(this.base.eventStartTime)
            this.base.eventEndTime = this.formDataDate(this.base.eventEndTime)

            var interfaceName = 'eventInfoInsert'

            if (this.$route.query.eventCode) {
              interfaceName = 'eventInfoUpdate'
            } else {
              this.base.eventDesigner = this.userInfo.userCode
            }

            this.base.enterpriseCode = this.$route.query.enterpriseCode
            
            util.request({
                method: 'post',
                interface: interfaceName,
                data: this.base
            }).then(res => {
                if (res.result.success == '1') {
                    this.$message({
                      type: 'success',
                      message: '保存成功!'
                    })

                    if (this.$route.query.eventCode) {
                      this.getBase()
                    } else {
                      var pathUrl = {
                        name: 'market-detail',
                        query: {
                          eventCode: res.result.result,
                          enterpriseCode: this.$route.query.enterpriseCode
                        }
                      }

                      this.$router.replace(pathUrl)
                    }
                    
                } else {
                    this.$message.error(res.result.message)
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