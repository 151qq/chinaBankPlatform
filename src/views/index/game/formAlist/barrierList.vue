<template>
    <section class="product-base-box">
      <el-button class="add-btn" type="primary"
                icon="plus" size="small"
                v-if="base.eventStatus == '1' || base.eventStatus == '2'"
                @click="addBarrier">
        增加
      </el-button>
      
      <section class="inForm" v-for="(item, index) in barrieList" :key="index">
        <div class="formDiscount">
          <section class="formBox">
            <span>关卡名称</span>
            <el-input
              class="input-box"
              placeholder="请输入内容,最20个字"
              :maxlength="20"
              v-model="item.gameGateName">
            </el-input>
          </section>
          <section class="formBox">
              <span>关卡序号</span>
              <el-input type="number" class="input-box" size="small" :min="1"
                  v-model="item.gameGateSequence"></el-input>
          </section>
          <section class="formBox">
              <span>通关时间(s)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.gateTimeLimit"></el-input>
          </section>
          <section class="formBox">
              <span>题目总数</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gateSubjectNumber"></el-input>
          </section>
          <section class="formBox">
              <span>题目高于(分)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.difficultSubjectRate"></el-input>
          </section>
          <section class="formBox">
              <span>高分题数</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.gateDifficultSubjectNumber"></el-input>
          </section>
          <section class="formBox">
              <span>题目低于(分)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.subjectEasyRate"></el-input>
          </section>
          <section class="formBox">
              <span>低分题数</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gateEasySubjectNumber"></el-input>
          </section>
          <section class="formBox">
              <span>通关阈值(%)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.gatePassRate"></el-input>
          </section>
          <section class="formBox">
              <span>挑战消耗积分</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gateConsumePoint"></el-input>
          </section>
          <section class="formBox">
              <span>允许作弊次数</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gateCheatNumber"></el-input>
          </section>
          <section class="formBox">
              <span>作弊消耗积分</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.cheatConsumePoint"></el-input>
          </section>
          <section class="formBox">
              <span>通关奖励类型</span>
              <el-select
                class="input-box"
                v-model="item.gatePassIncentiveType"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in gatePassIncentiveTypes"
                  :key="index"
                  :label="item.key"
                  :value="item.value">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gatePassIncentiveType == '1'">
              <span>奖励卡券</span>
              <el-select
                class="input-box"
                v-model="item.gatePassCoupon"
                filterable
                placeholder="请选择">
                <el-option
                  :key="index"
                  :label="'无'"
                  :value="''">
                </el-option>
                <el-option
                  v-for="(coupon, index) in couponList"
                  v-if="coupon.couponCode != item.gatePassCoupon && coupon.couponCode != item.gateFailCoupon"
                  :label="coupon.couponTitle"
                  :value="coupon.couponCode">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gatePassIncentiveType == '0'">
              <span>奖励金额(元)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.gatePassCash"></el-input>
          </section>

          <section class="formBox">
              <span>通关积分</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gatePassPoint"></el-input>
          </section>

          <section class="formBox">
              <span>失败安慰类型</span>
              <el-select
                class="input-box"
                v-model="item.gateFailIncentiveType"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in gatePassIncentiveTypes"
                  :key="index"
                  :label="item.key"
                  :value="item.value">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gateFailIncentiveType == '1'">
              <span>安慰鼓励</span>
              <el-select
                class="input-box"
                v-model="item.gateFailCoupon"
                filterable
                placeholder="请选择">
                <el-option
                  :key="index"
                  :label="'无'"
                  :value="''">
                </el-option>
                <el-option
                  v-for="(coupon, index) in couponList"
                  v-if="coupon.couponCode != item.gatePassCoupon && coupon.couponCode != item.gateFailCoupon"
                  :label="coupon.couponTitle"
                  :value="coupon.couponCode">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gateFailIncentiveType == '0'">
              <span>安慰鼓励(元)</span>
              <el-input type="number" class="input-box" size="small" :min="15" :max="60"
                  v-model="item.gateFailCash"></el-input>
          </section>

          <section class="formBox">
              <span>安慰积分</span>
              <el-input type="number" class="input-box" size="small" :min="0"
                  v-model="item.gateFailPoit"></el-input>
          </section>
          <section class="formBox">
              <span>关卡题材</span>
              <el-select
                class="input-box"
                v-model="item.gateSubjectTag"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in tagList.content_type"
                  :key="index"
                  :label="item.tagValueCname"
                  :value="item.tagValue">
                </el-option>
              </el-select>
          </section>
          <section class="formBox">
            <span>成功者称号</span>
            <el-input
              class="input-box"
              placeholder="请输入内容,最20个字"
              :maxlength="20"
              v-model="item.gatePassNickname">
            </el-input>
          </section>
          <section class="formBox">
            <span>失败者称号</span>
            <el-input
              class="input-box"
              placeholder="请输入内容,最20个字"
              :maxlength="20"
              v-model="item.gateFailNickname">
            </el-input>
          </section>
          <div class="clear"></div>
          <section class="formBox">
            <span>成功者徽章</span>
            <div class="input-box">
              <upload :path="item.gateFailImg"
                      :bg-path="false"
                      :is-operate="isEdit"
                      :item-index="index"
                      @changeImg="changeFailImg"></upload>
            </div>
          </section>
          <section class="formBox">
            <span>失败者徽章</span>
            <div class="input-box">
              <upload :path="item.gatePassImg"
                      :bg-path="false"
                      :is-operate="isEdit"
                      :item-index="index"
                      @changeImg="changePassImg"></upload>
            </div>
          </section>       
        </div>
        <el-button v-if="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')" class="save-btn" type="danger" :plain="true" size="small" icon="delete2"
            @click="deleteBase(item, index)">删除</el-button>

        <el-button v-if="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')" class="save-btn" type="info" :plain="true" size="small" icon="document"
            @click="saveBase(item)">保存</el-button>
        <div class="clear"></div>
      </section>

      <div v-if="!barrieList.length" class="null-page">
            暂无关卡
      </div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import upload from '../../../../components/common/upload'
import { mapGetters } from 'vuex'
export default {
    props: ['base', 'couponList', 'gameCode'],
    data () {
        return {
            barrieList: [],
            isOperate: true,
            gatePassIncentiveTypes: [
              {
                value: '0',
                key: '现金'
              },
              {
                value: '1',
                key: '套券'
              },
              {
                value: '3',
                key: '无'
              }
            ],
            tagList: {
                age: [],
                content_type: [],
                consume_level: [],
                education: [],
                gender: [],
                shopping_guide: [],
                career_type: [],
                unit_type: [],
                industry_type: [],
                content_politics: []
            }
        }
    },
    mounted () {
      this.getList()
      this.getTags()
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
        changeFailImg (data) {
          this.barrieList[data.index].gateFailImg = data.url
        },
        changePassImg (data) {
          this.barrieList[data.index].gatePassImg = data.url
        },
        getTags () {
            util.request({
                method: 'get',
                interface: 'selectAllTagDef',
                data: {}
            }).then(res => {
                if (res.result.success == '1') {
                    this.tagList = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getList () {
          util.request({
              method: 'get',
              interface: 'getGameGateListByCode',
              data: {
                gameCode: this.gameCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.barrieList = res.result.result
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        addBarrier () {
          this.barrieList.push({
            gameCode: this.gameCode,
            gameGateSequence: '',
            gameGateCode: '',
            gameGateName: '',
            gateTimeLimit: '',
            gateSubjectNumber: '',
            gateDifficultSubjectNumber: '',
            gateEasySubjectNumber: '',
            gateConsumePoint: '',
            gatePassRate: '',
            gateCheatNumber: '',
            cheatConsumePoint: '',
            gatePassIncentiveType: '1',
            gatePassCash: '',
            gatePassCoupon: '',
            gatePassPoint: '',
            gatePassNickname: '',
            gateFailIncentiveType: '1',
            gateFailCash: '',
            gateFailCoupon: '',
            gateFailPoit: '',
            gateSubjectTag: '',
            gateFailNickname: '',
            difficultSubjectRate: '',
            subjectEasyRate: '',
            gateFailImg: '',
            gatePassImg: ''
          })
        },
        deleteBase (barrieData, index) {
          if (!barrieData.code) {
            this.barrieList.splice(index, 1)
          } else {
            util.request({
                method: 'get',
                interface: 'gameGateDelete',
                data: {
                  gameGateCode: barrieData.gameGateCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.getList()
                } else {
                    this.$message.error(res.result.message)
                }
            })
          }
        },
        saveBase (barrieData) {
            if (barrieData.gameGateName.trim() == '') {
                this.$message({
                    message: '请填写关卡名称！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gameGateSequence) {
                this.$message({
                    message: '请填写关卡序号！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gateTimeLimit) {
                this.$message({
                    message: '请填写通关时间！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateSubjectNumber == '') {
                this.$message({
                    message: '请填写题目总数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.difficultSubjectRate == '') {
                this.$message({
                    message: '请填写题目高于分数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateDifficultSubjectNumber == '') {
                this.$message({
                    message: '请填写高分题数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.subjectEasyRate == '') {
                this.$message({
                    message: '请填写题目低于分数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.subjectEasyRate > barrieData.difficultSubjectRate) {
                this.$message({
                    message: '高分数必须大于低分数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateEasySubjectNumber == '') {
                this.$message({
                    message: '请填写低分题数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateSubjectNumber < barrieData.gateDifficultSubjectNumber + barrieData.gateEasySubjectNumber) {
                this.$message({
                    message: '题数数量分配不对！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gatePassRate) {
                this.$message({
                    message: '请填写通关阈值！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gateConsumePoint) {
                this.$message({
                    message: '请填写挑战消耗几分！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gateCheatNumber) {
                this.$message({
                    message: '请填写允许作弊次数！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.cheatConsumePoint) {
                this.$message({
                    message: '请填写作弊消耗积分！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType == '0' && !barrieData.gatePassCash) {
                this.$message({
                    message: '请填写奖励金额！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType == '1' && !barrieData.gatePassCoupon) {
                this.$message({
                    message: '请选择奖励套券！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassPoint == '') {
                this.$message({
                    message: '请填写通关积分！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType == '0' && !barrieData.gateFailCash) {
                this.$message({
                    message: '请填写安慰金额！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType == '1' && !barrieData.gateFailCoupon) {
                this.$message({
                    message: '请选择安慰套券！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailPoit == '') {
                this.$message({
                    message: '请填写安慰积分！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gateSubjectTag) {
                this.$message({
                    message: '请选择关卡题材！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gatePassNickname) {
                this.$message({
                    message: '请选择成功者称号！',
                    type: 'warning'
                })
                return false
            }

            if (!barrieData.gateFailNickname) {
                this.$message({
                    message: '请选择失败者称号！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType == '3') {
                barrieData.gatePassCoupon = ''
                barrieData.gatePassCash = ''
            }

            if (barrieData.gatePassIncentiveType == '0') {
                barrieData.gatePassCoupon = ''
            }

            if (barrieData.gatePassIncentiveType == '1') {
                barrieData.gatePassCash = ''
            }

            if (barrieData.gateFailIncentiveType == '3') {
                barrieData.gateFailCoupon = ''
                barrieData.gateFailCash = ''
            }

            if (barrieData.gateFailIncentiveType == '0') {
                barrieData.gateFailCoupon = ''
            }

            if (barrieData.gateFailIncentiveType == '1') {
                barrieData.gateFailCash = ''
            }
            
            util.request({
                method: 'post',
                interface: 'gameGateOperate',
                data: barrieData
            }).then(res => {
                if (res.result.success == '1') {
                    this.$message({
                      type: 'success',
                      message: '保存成功!'
                    })

                    this.getList()
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

  .inForm {
    padding: 20px 0;
    border-bottom: 1px solid #99A9BF;

    &:last-child {
      border: none;
      padding-bottom: 0;
    }

    &:nth-child(2) {
      padding-top: 0;
    }
  }
}
</style>