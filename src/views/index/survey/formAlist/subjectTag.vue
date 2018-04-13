<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox">
            <span>题目适用年龄</span>
            <el-select
              class="input-box"
              v-model="base.age"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.age"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适用行业</span>
            <el-select
              class="input-box"
              v-model="base.industry_type"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.industry_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目所属题材</span>
            <el-select
              class="input-box"
              v-model="base.age"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.age"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适用性别</span>
            <el-select
              class="input-box"
              v-model="base.gender"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.gender"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适用学历</span>
            <el-select
              class="input-box"
              v-model="base.education"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.education"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适用阶层</span>
            <el-select
              class="input-box"
              v-model="base.consume_level"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.consume_level"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适合职业</span>
            <el-select
              class="input-box"
              v-model="base.career_type"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.career_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目政治倾向</span>
            <el-select
              class="input-box"
              v-model="base.content_politics"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.content_politics"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目引导倾向</span>
            <el-select
              class="input-box"
              v-model="base.shopping_guide"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.shopping_guide"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox">
            <span>题目适合企业</span>
            <el-select
              class="input-box"
              v-model="base.unit_type"
              filterable
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.unit_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
      </div>
      <el-button class="save-btn" type="info" :plain="true" size="small" icon="document"
          @click="saveBase()">保存</el-button>
      <div class="clear"></div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
export default {
    data () {
        return {
            base: {
              age: '',
              consume_decision_type: '',
              consume_level: '',
              education: '',
              gender: '',
              shopping_guide: '',
              career_type: '',
              unit_type: '',
              industry_type: '',
              content_politics: ''
            },
            tagList: {
                age: [],
                consume_decision_type: [],
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
      // this.getTags()
      // this.getBase()
    },
    methods: {
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
        }
    }
}
</script>