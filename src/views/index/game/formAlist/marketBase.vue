<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox bigF">
            <span>游戏名称</span>
            <div class="input-box">
              <ueditor :editor-id="'gameTitle'"
                      :editor-type="'editText'"
                      :index="'eventPlanTitle'"
                      :content="base.eventPlanTitle"
                      @setContent="setContent"></ueditor>
            </div>
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
            <span>游戏说明</span>
            <div class="input-box">
              <ueditor :editor-id="'gameDesc'"
                      :editor-type="'editText'"
                      :index="'eventPlanDesc'"
                      :content="base.eventPlanDesc"
                      @setContent="setContent"></ueditor>
            </div>
        </section>
        <section class="formBox">
          <span>游戏封面</span>
          <div class="input-box">
            <upload :path="base.eventPlanCover"
                    :bg-path="false"
                    :is-operate="true"
                    @changeImg="changeImg"></upload>
          </div>
        </section>
      </div>
      <router-link class="save-btn"
                  target="_blank"
                  :to="{
                    name: 'game-statistic',
                    query: {
                      enterpriseCode: $route.query.enterpriseCode,
                      eventCode: $route.query.eventCode,
                      beginDate: formDataDate(base.eventStartTime),
                      endDate: formDataDate(base.eventEndTime)
                    }
                  }">
        <el-button v-if="base.eventStatus == '3'" type="info" :plain="true" size="small" icon="share">统计</el-button>
      </router-link>

      <el-button v-if="isEdit" class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase()">保存</el-button>
      <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import ueditor from '../../../../components/common/ueditor'
import upload from '../../../../components/common/upload'
import { mapGetters } from 'vuex'
export default {
    data () {
        return {
            base: {
              eventPlanTitle: '',
              eventStartTime: '',
              eventEndTime: '',
              eventType: '2',
              eventStatus: '1',
              eventLeads: '',
              eventHotLeads: '',
              eventSalesOpp: '',
              eventPlanDesc: '',
              eventPlanCover: ''
            },
            statusTypes: [],
            isOperate: true,
            pickerOptions: {
              disabledDate(time) {
                return time.getTime() < Date.now() - 3600 * 1000 * 24
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
        formDataDate (date) {
          return util.formDataDate(date)
        },
        setContent (data) {
            this.base[data.index] = data.content
        },
        getBase () {
          util.request({
              method: 'get',
              interface: 'eventInfoGet',
              data: {
                eventCode: this.$route.query.eventCode
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
                    message: '请填写游戏名称！',
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

            this.base.eventStartTime = this.formDataDate(this.base.eventStartTime) + ' 23:59:59'
            this.base.eventEndTime = this.formDataDate(this.base.eventEndTime) + ' 00:00:00'

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
                        name: 'game-detail',
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
      upload,
      ueditor
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