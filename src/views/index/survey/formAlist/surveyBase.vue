<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox">
            <span>题目适用场景</span>
            <el-select
              class="input-box"
              v-model="base.subjectScenario"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in selectList.scenario"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目分值</span>
            <el-input
              class="input-box"
              type="number"
              :min="1"
              :max="9999"
              placeholder="请输入内容"
              v-model="base.subjectValue">
            </el-input>
        </section>
        <section class="formBox">
            <span>题目描述约束</span>
            <el-select
              class="input-box"
              v-model="base.sujectCSStype"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in selectList.sujectTypes"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>选项描述约束</span>
            <el-select
              class="input-box"
              v-model="base.optionCSSType"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in selectList.optionTypes"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>选项类型</span>
            <el-select
              class="input-box"
              v-model="base.subjectChooseType"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in selectList.chooseTypes"
                :key="index"
                :label="item.key"
                :value="item.value">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目编号</span>
            <el-input
              class="input-box"
              type="number"
              :min="0"
              placeholder="请输入内容"
              v-model="base.subjectSequence">
            </el-input>
        </section>
        <section class="formBox bigF">
            <span>题目</span>
            <el-input
              type="textarea"
              :rows="4"
              :maxlength="200"
              placeholder="请输入内容"
              v-model="base.subjectContent">
            </el-input>
        </section>
        <section class="formBox bigF">
          <span>题目图片</span>
          <div class="input-box">
            <small-imgs :attachment-data="base.attachment" :is-operate="true"
                        :attachment-type="'subjectQustion'"></small-imgs>
          </div>
        </section>
        <section class="formBox bigF">
          <span class="btn-color" @click="addAttachment">添加附件</span>
          <div class="input-box">
            <article-list :article-list="base.articleList" @deleteArticle="deleteArticle"
                          :is-edit="true"></article-list>
          </div>
        </section>
      </div>
      <el-button class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase()">保存</el-button>
      <div class="clear"></div>
      <attachment-article :visible-data="visibleData" :article-code="base.formData.attachmentSourceCodes"
                          :articleList="base.articleList"
                          @getAttachments="setAttachments"
                          :ref="'question'"></attachment-article>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import smallImgs from '../../../../components/common/small-imgs'
import attachmentArticle from '../../../../components/common/attachment-article'
import articleList from './articleList'
export default {
    data () {
        return {
            base: {
              subjectScenario: '1',
              subjectValue: '',
              optionCSSType: '1',
              sujectCSStype: '1',
              tagCodes: [],
              subjectChooseType: '1',
              subjectSequence: '',
              subjectContent: '',
              attachment: [],
              formData: {
                attachmentTargetType: 'subjectQustion',
                attachmentSourceType: "attachmen_type_4",
                attachmentSourceCodes: []
              },
              articleList: []
            },
            selectList: {
              scenario: [
                {
                  value: '1',
                  key: '答题游戏'
                },
                {
                  value: '2',
                  key: '营销培训'
                },
                {
                  value: '3',
                  key: '在线课程'
                }
              ],
              sujectTypes: [
                {
                  value: '1',
                  key: '复杂样式'
                },
                {
                  value: '2',
                  key: '简单样式' // 文本+一张图片
                }
              ],
              optionTypes: [
                {
                  value: '1',
                  key: '复杂样式'
                },
                {
                  value: '2',
                  key: '简单文本'
                },
                {
                  value: '3',
                  key: '简单图片'
                }
              ],
              chooseTypes: [
                {
                  value: '1',
                  key: '单选'
                },
                {
                  value: '0',
                  key: '多选'
                },
                {
                  value: '2',
                  key: '单选可添加'
                },
                {
                  value: '3',
                  key: '多选可添加'
                }
              ]
            },
            visibleData: {
              show: false
            }
        }
    },
    mounted () {
      // this.getBase()
    },
    watch: {
      $route () {
        // this.getBase()
      }
    },
    methods: {
        getBase () {
          util.request({
              method: 'get',
              interface: 'selectSurveyInfo',
              data: {
                surveyCode: this.$route.query.surveyCode
              }
          }).then(res => {
              this.base = res.result.result
          })
        },
        saveBase () {
            if (!this.base.surveyTitle) {
                this.$message({
                    message: '请填写调研标题！',
                    type: 'warning'
                })
                return false
            }
            
            util.request({
                method: 'post',
                interface: 'manageSurveyInfo',
                data: this.base
            }).then(res => {
                if (res.result.success == '1') {
                    this.getBase()

                    this.$message({
                      type: 'success',
                      message: '保存成功!'
                    })
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        addAttachment () {
          this.visibleData.show = true
          this.$refs['question'].initData()
        },
        setAttachments (articleData) {
          this.base.articleList = [].concat(articleData.datas)
          this.base.formData.attachmentSourceCodes = [].concat(articleData.codes)
        },
        deleteArticle (data) {
          this.base.articleList.splice(index, 1)
          this.base.formData.attachmentSourceCodes.splice(index, 1)
        }
    },
    components: {
      smallImgs,
      attachmentArticle,
      articleList
    }
}
</script>