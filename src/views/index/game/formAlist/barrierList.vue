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
              <el-input-number class="input-box" size="small" :min="1"
                  v-model="item.gameGateSequence"></el-input-number>
          </section>
          <section class="formBox">
              <span>通关时间(s)</span>
              <el-input-number class="input-box" size="small" :min="15"
                  v-model="item.gateTimeLimit"></el-input-number>
          </section>
          <section class="formBox">
              <span>题目总数</span>
              <el-input-number class="input-box" size="small" :min="1"
                  v-model="item.gateSubjectNumber"></el-input-number>
          </section>
          <section class="formBox">
              <span>题目高于(分)</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.difficultSubjectRate"></el-input-number>
          </section>
          <section class="formBox">
              <span>高分题数</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateDifficultSubjectNumber"></el-input-number>
          </section>
          <section class="formBox">
              <span>题目低于(分)</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.subjectEasyRate"></el-input-number>
          </section>
          <section class="formBox">
              <span>低分题数</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateEasySubjectNumber"></el-input-number>
          </section>
          <section class="formBox">
              <span>通关阈值(%)</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gatePassRate"></el-input-number>
          </section>
          <section class="formBox">
              <span>挑战消耗积分</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateConsumePoint"></el-input-number>
          </section>
          <section class="formBox">
              <span>允许作弊次数</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateCheatNumber"></el-input-number>
          </section>
          <section class="formBox">
              <span>作弊消耗积分</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.cheatConsumePoint"></el-input-number>
          </section>
          <section class="formBox">
              <span>通关奖励类型</span>
              <el-select
                class="input-box"
                v-model="item.gatePassIncentiveType"
                :disabled="!isEdit"
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
                :disabled="!isEdit"
                placeholder="请选择"
                @change="couponChange">
                <el-option
                  :key="index"
                  :label="'无'"
                  :value="''">
                </el-option>
                <el-option
                  v-for="(coupon, index) in couponList"
                  v-if="hasSelectCoupons.indexOf(coupon.couponCode) < 0 || coupon.couponCode == item.gatePassCoupon"
                  :label="coupon.couponTitle"
                  :value="coupon.couponCode">
                </el-option>
              </el-select>
          </section>
          <section class="formBox" v-if="item.gatePassIncentiveType == '2'">
              <span>奖励礼品</span>
              <el-select
                class="input-box"
                v-model="item.gatePassGift"
                filterable
                :disabled="!isEdit"
                placeholder="请选择">
                <el-option
                  v-for="(gift, index) in giftLists"
                  :label="gift.productCname"
                  :value="gift.productCode">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gatePassIncentiveType == '0'">
              <span>奖励金额(元)</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gatePassCash"></el-input-number>
          </section>

          <section class="formBox" v-if="item.gatePassIncentiveType != '3'">
              <span>通关取奖类型</span>
              <el-select
                class="input-box"
                v-model="item.gatePassAwardType"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in gatePassAwardTypes"
                  :key="index"
                  :label="item.key"
                  :value="item.value">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gatePassAwardType == '2' && item.gatePassIncentiveType != '3'">
              <span>通关中奖序号</span>
              <el-input-number class="input-box" size="small" :min="1"
                  v-model="item.gatePassAveragePlayer"></el-input-number>
          </section>

          <section class="formBox" v-if="item.gatePassAwardType == '2' && item.gatePassIncentiveType != '3'">
              <span>最大通关中奖数</span>
              <el-input-number class="input-box" size="small" :min="1"
                  :disabled="!isEdit"
                  v-model="item.gatePassGiftSum"></el-input-number>
          </section>

          <section class="formBox">
              <span>通关积分</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gatePassPoint"></el-input-number>
          </section>

          <section class="formBox">
              <span>失败安慰类型</span>
              <el-select
                class="input-box"
                v-model="item.gateFailIncentiveType"
                :disabled="!isEdit"
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
                :disabled="!isEdit"
                filterable
                placeholder="请选择"
                @change="couponChange">
                <el-option
                  :key="index"
                  :label="'无'"
                  :value="''">
                </el-option>
                <el-option
                  v-for="(coupon, index) in couponList"
                  v-if="hasSelectCoupons.indexOf(coupon.couponCode) < 0 || coupon.couponCode == item.gateFailCoupon"
                  :label="coupon.couponTitle"
                  :value="coupon.couponCode">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gateFailIncentiveType == '2'">
              <span>安慰礼品</span>
              <el-select
                class="input-box"
                v-model="item.gateFailGift"
                filterable
                :disabled="!isEdit"
                placeholder="请选择">
                <el-option
                  v-for="(gift, index) in giftLists"
                  :label="gift.productCname"
                  :value="gift.productCode">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gateFailIncentiveType == '0'">
              <span>安慰鼓励(元)</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateFailCash"></el-input-number>
          </section>

          <section class="formBox" v-if="item.gateFailIncentiveType != '3'">
              <span>安慰取奖类型</span>
              <el-select
                class="input-box"
                v-model="item.gateFailAwardType"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in gatePassAwardTypes"
                  :key="index"
                  :label="item.key"
                  :value="item.value">
                </el-option>
              </el-select>
          </section>

          <section class="formBox" v-if="item.gateFailAwardType == '2' && item.gateFailIncentiveType != '3'">
              <span>安慰中奖序号</span>
              <el-input-number class="input-box" size="small" :min="1"
                  v-model="item.gateFailAveragePlayer"></el-input-number>
          </section>

          <section class="formBox" v-if="item.gateFailAwardType == '2' && item.gateFailIncentiveType != '3'">
              <span>最大安慰中奖数</span>
              <el-input-number class="input-box" size="small" :min="1"
                  :disabled="!isEdit"
                  v-model="item.gateFailGiftSum"></el-input-number>
          </section>

          <section class="formBox">
              <span>安慰积分</span>
              <el-input-number class="input-box" size="small" :min="0"
                  v-model="item.gateFailPoit"></el-input-number>
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
        </div>
        <router-link class="save-btn"
                    target="_blank"
                    :to="{
                      name: 'gate-statistic',
                      query: {
                        enterpriseCode: $route.query.enterpriseCode,
                        eventCode: $route.query.eventCode,
                        gameCode: item.gameCode,
                        gameGateCode: item.gameGateCode
                      }
                    }">
          <el-button v-if="base.eventStatus == '3'" type="info" :plain="true" size="small" icon="share">统计</el-button>
        </router-link>

        <el-button v-if="isEdit" class="save-btn" type="danger" :plain="true" size="small" icon="delete2"
            @click="deleteBase(item, index)">删除</el-button>

        <el-button class="save-btn" type="info" :plain="true" size="small" icon="document"
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
                value: '2',
                key: '礼品'
              },
              {
                value: '3',
                key: '无'
              }
            ],
            gatePassAwardTypes: [
              {
                value: '1',
                key: '兑奖'
              },
              {
                value: '2',
                key: '摇奖'
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
            },
            hasSelectCoupons: [],
            giftLists: []
        }
    },
    mounted () {
      this.getList()
      this.getTags()
      this.getGifts()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          return this.$route.query.enterpriseCode == this.userInfo.enterpriseCode && (this.base.eventStatus == '1' || this.base.eventStatus == '2')
        }
    },
    methods: {
        formDataDate (date) {
          return util.formDataDate(date)
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
                this.resetCoupons()
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        couponChange () {
          this.resetCoupons()
        },
        resetCoupons () {
          var arrs = []
          this.barrieList.forEach((item) => {
            if (item.gatePassCoupon) {
              arrs.push(item.gatePassCoupon)
            }

            if (item.gateFailCoupon) {
              arrs.push(item.gateFailCoupon)
            }
          })
          this.hasSelectCoupons = arrs
        },
        getGifts () {
            util.request({
                method: 'get',
                interface: 'gifProductList',
                data: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    productType: 'gift'
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.giftLists = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })       
        },
        addBarrier () {
          this.barrieList.unshift({
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
            gatePassGift: '',
            gatePassAwardType: '2',
            gatePassAveragePlayer: '',
            gatePassGiftSum: '',
            gatePassPoint: '',
            gatePassNickname: '',
            gateFailIncentiveType: '1',
            gateFailCash: '',
            gateFailCoupon: '',
            gateFailGift: '',
            gateFailAwardType: '2',
            gateFailAveragePlayer: '',
            gateFailGiftSum: '',
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
          if (!barrieData.gameGateCode) {
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
            if (barrieData.gameGateName.trim() === '') {
                this.$message({
                    message: '请填写关卡名称！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gameGateSequence === '') {
                this.$message({
                    message: '请填写关卡序号！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateTimeLimit === '') {
                this.$message({
                    message: '请填写通关时间！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateSubjectNumber === '') {
                this.$message({
                    message: '请填写题目总数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.difficultSubjectRate === '') {
                this.$message({
                    message: '请填写题目高于分数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateDifficultSubjectNumber === '') {
                this.$message({
                    message: '请填写高分题数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.subjectEasyRate === '') {
                this.$message({
                    message: '请填写题目低于分数！',
                    type: 'warning'
                })
                return false
            }

            if (Number(barrieData.subjectEasyRate) > Number(barrieData.difficultSubjectRate)) {
                this.$message({
                    message: '高分数必须大于低分数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateEasySubjectNumber === '') {
                this.$message({
                    message: '请填写低分题数！',
                    type: 'warning'
                })
                return false
            }

            if (Number(barrieData.gateSubjectNumber) < Number(barrieData.gateDifficultSubjectNumber) + Number(barrieData.gateEasySubjectNumber)) {
                this.$message({
                    message: '题数数量分配不对！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassRate === '') {
                this.$message({
                    message: '请填写通关阈值！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateConsumePoint === '') {
                this.$message({
                    message: '请填写挑战消耗几分！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateCheatNumber === '') {
                this.$message({
                    message: '请填写允许作弊次数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.cheatConsumePoint === '') {
                this.$message({
                    message: '请填写作弊消耗积分！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType === '0' && !barrieData.gatePassCash) {
                this.$message({
                    message: '请填写奖励金额！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType === '1' && !barrieData.gatePassCoupon) {
                this.$message({
                    message: '请选择奖励套券！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType === '2' && !barrieData.gatePassGift) {
                this.$message({
                    message: '请选择奖励礼品！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassPoint === '') {
                this.$message({
                    message: '请填写通关积分！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType === '0' && !barrieData.gateFailCash) {
                this.$message({
                    message: '请填写安慰金额！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType === '1' && !barrieData.gateFailCoupon) {
                this.$message({
                    message: '请选择安慰套券！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType === '2' && !barrieData.gateFailGift) {
                this.$message({
                    message: '请选择奖励礼品！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType !== '3' && barrieData.gatePassAwardType === '2' && !barrieData.gatePassAveragePlayer) {
                this.$message({
                    message: '请填写通关中奖序号！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gatePassIncentiveType !== '3' && barrieData.gatePassAwardType === '2' && !barrieData.gatePassGiftSum) {
                this.$message({
                    message: '请填写最大通关中奖数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType !== '3' && barrieData.gateFailAwardType === '2' && !barrieData.gateFailAveragePlayer) {
                this.$message({
                    message: '请填写安慰中奖序号！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailIncentiveType !== '3' && barrieData.gateFailAwardType === '2' && !barrieData.gateFailGiftSum) {
                this.$message({
                    message: '请填写最大安慰中奖数！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.gateFailPoit === '') {
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

            if (barrieData.gatePassIncentiveType === '3') {
                barrieData.gatePassCoupon = ''
                barrieData.gatePassCash = ''
                barrieData.gatePassGift = ''
            }

            if (barrieData.gatePassIncentiveType === '0') {
                barrieData.gatePassCoupon = ''
                barrieData.gatePassGift = ''
            }

            if (barrieData.gatePassIncentiveType === '1') {
                barrieData.gatePassCash = ''
                barrieData.gatePassGift = ''
            }

            if (barrieData.gatePassIncentiveType === '2') {
                barrieData.gatePassCoupon = ''
                barrieData.gatePassCash = ''
            }

            if (barrieData.gateFailIncentiveType === '3') {
                barrieData.gateFailCoupon = ''
                barrieData.gateFailCash = ''
                barrieData.gateFailGift = ''
            }

            if (barrieData.gateFailIncentiveType === '0') {
                barrieData.gateFailCoupon = ''
                barrieData.gateFailGift = ''
            }

            if (barrieData.gateFailIncentiveType === '1') {
                barrieData.gateFailCash = ''
                barrieData.gateFailGift = ''
            }

            if (barrieData.gateFailIncentiveType === '2') {
                barrieData.gateFailCoupon = ''
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

    &:nth-child(1) {
      padding-top: 0;
    }
  }
}
</style>