<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox">
            <span>分享积分</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gameSharePoint"></el-input>
        </section>
        <section class="formBox">
            <span>获客积分</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gameNewCustomerPoint"></el-input>
        </section>
        <section class="formBox">
            <span>玩游戏条件</span>
            <el-select
              class="input-box"
              v-model="game.gamePlayPrecondition"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in gamePlayPreconditions"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>奖励累积规则</span>
            <el-select
              class="input-box"
              v-model="game.gameIncentiveAccumulateRule"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in gameIncentiveAccumulateRules"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>冲关失败退回</span>
            <el-select
              class="input-box"
              v-model="game.gameFailureBack"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in gameFailureBacks"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>PK有效阈值(%)</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gamePkEffectiveRate"></el-input>
        </section>
        <section class="formBox">
            <span>PK最少押分</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gamePkMinBet"></el-input>
        </section>
        <section class="formBox">
            <span>PK赛时间(s)</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gamePkTime"></el-input>
        </section>
        <section class="formBox">
            <span>PK赛目总数</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.gamePkSubjectNumber"></el-input>
        </section>
        <section class="formBox">
            <span>题目高于(分)</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.pkDifficultSubjectRate"></el-input>
        </section>
        <section class="formBox">
            <span>低分题数</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.pkGateDifficultSubjectNumber"></el-input>
        </section>
        <section class="formBox">
            <span>题目低于(分)</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.pkSubjectEasyRate"></el-input>
        </section>
        <section class="formBox">
            <span>高分题数</span>
            <el-input type="number" class="input-box" size="small" :min="0"
                v-model="game.pkGateEasySubjectNumber"></el-input>
        </section>
        
        
        
        
        
        
      </div>
      <el-button v-if="isEdit && (base.eventStatus == '1' || base.eventStatus == '2')" class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase()">保存</el-button>
      <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['base'],
    data () {
        return {
            isOperate: true,
            game: {
                gameSharePoint: '',
                gameNewCustomerPoint: '',
                gameIncentiveAccumulateRule: '1',
                gameFailureBack: '1',
                gamePlayPrecondition: '1',
                gamePkEffectiveRate: '',
                gamePkMinBet: '',
                gamePkTime: '',
                gamePkSubjectNumber: '',
                pkDifficultSubjectRate: '',
                pkGateDifficultSubjectNumber: '',
                pkSubjectEasyRate: '',
                pkGateEasySubjectNumber: ''
            },
            gameIncentiveAccumulateRules: [
                {
                    value: '0',
                    key: '不积累'
                },
                {
                    value: '1',
                    key: '积累'
                }
            ],
            gameFailureBacks: [
              {
                value: '1',
                key: '回原点'
              },
              {
                value: '2',
                key: '在本关'
              }
            ],
            gamePlayPreconditions: [
                {
                    value: '0',
                    key: '无条件'
                },
                {
                    value: '1',
                    key: '必须先注册'
                },
                {
                    value: '2',
                    key: '必须先关注'
                },
                {
                    value: '3',
                    key: '必须先关注后注册'
                }
            ]
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
    mounted () {
        this.getGame()
    },
    methods: {
        getGame () {
            util.request({
                method: 'get',
                interface: 'getGameInfoByCode',
                data: {
                    eventCode: this.$route.query.eventCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    if (res.result.result.length) {
                        this.gameDetail = res.result.result[0]
                        this.$emit('hasGame', this.gameDetail)
                    }
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        saveBase () {
            if (this.game.gameSharePoint == '') {
                this.$message({
                    message: '请填写分享积分！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.gameNewCustomerPoint == '') {
                this.$message({
                    message: '请填写获客积分！',
                    type: 'warning'
                })
                return false
            }

            
            if (this.game.gamePkEffectiveRate == '') {
                this.$message({
                    message: '请填写PK有效阈值！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.gamePkMinBet == '') {
                this.$message({
                    message: '请填写PK最少押分！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.gamePkTime == '') {
                this.$message({
                    message: '请填写PK赛时间！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.gamePkSubjectNumber == '') {
                this.$message({
                    message: '请填写PK赛目总数！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.pkDifficultSubjectRate == '') {
                this.$message({
                    message: '请填写题目高于分值！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.pkGateDifficultSubjectNumber == '') {
                this.$message({
                    message: '请填写高分题数目！',
                    type: 'warning'
                })
                return false
            }


            if (this.game.pkSubjectEasyRate == '') {
                this.$message({
                    message: '请填写题目低于分值！',
                    type: 'warning'
                })
                return false
            }

            if (this.game.pkGateEasySubjectNumber == '') {
                this.$message({
                    message: '请填写低分题数目！',
                    type: 'warning'
                })
                return false
            }

            this.game.enterpriseCode = this.$route.query.enterpriseCode
            this.game.eventCode = this.$route.query.eventCode
            
            util.request({
                method: 'post',
                interface: 'gameInfoOperate',
                data: this.game
            }).then(res => {
                if (res.result.success == '1') {
                    this.$message({
                      type: 'success',
                      message: '保存成功!'
                    })

                    this.getGame()
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
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