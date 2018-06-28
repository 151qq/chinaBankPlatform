<template>
    <section class="product-base-box">
      <div class="formDiscount">
        <section class="formBox bigF">
            <span>题目适用场景</span>
            <el-select
              class="input-box"
              v-model="base.subject_scenario"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.subject_scenario"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适用年龄</span>
            <el-select
              class="input-box"
              v-model="base.age"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.age"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目所属题材</span>
            <el-select
              class="input-box"
              v-model="base.content_type"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.content_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适用性别</span>
            <el-select
              class="input-box"
              v-model="base.gender"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.gender"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适用学历</span>
            <el-select
              class="input-box"
              v-model="base.education"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.education"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适用行业</span>
            <el-select
              class="input-box"
              v-model="base.industry_type"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.industry_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适用阶层</span>
            <el-select
              class="input-box"
              v-model="base.consume_level"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.consume_level"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适合职业</span>
            <el-select
              class="input-box"
              v-model="base.career_type"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.career_type"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目政治倾向</span>
            <el-select
              class="input-box"
              v-model="base.content_politics"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.content_politics"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目引导倾向</span>
            <el-select
              class="input-box"
              v-model="base.shopping_guide"
              filterable
              multiple
              placeholder="请选择">
              <el-option
                v-for="(item, index) in tagList.shopping_guide"
                :key="index"
                :label="item.tagValueCname"
                :value="item.tagValue">
              </el-option>
            </el-select>
        </section>
        <section class="formBox bigF">
            <span>题目适合企业</span>
            <el-select
              class="input-box"
              v-model="base.unit_type"
              filterable
              multiple
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
              enterpriseCode: '',
              subjectCode: '',
              subject_scenario: [],
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
                content_politics: [],
                subject_scenario: []
            }
        }
    },
    mounted () {
      this.getTags()
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
                    this.getBase()
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getBase () {
          util.request({
              method: 'get',
              interface: 'getRelation',
              data: {
                enterpriseCode: this.$route.query.enterpriseCode,
                subjectCode: this.$route.query.subjectCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                for (var key in res.result.result) {
                  if (key != 'enterpriseCode' && key != 'subjectCode' ) {
                    if (res.result.result[key]) {
                      res.result.result[key] = res.result.result[key].split(',')
                    } else {
                      res.result.result[key] = []
                    }
                  }
                }

                this.base = Object.assign(this.base, res.result.result)
              }
          })
        },
        saveBase () {
            if (!this.base.subject_scenario.length) {
              this.$message({
                  message: '请填写题目适用场景！',
                  type: 'warning'
              })
              return false
            }

            if (!this.base.age.length) {
              this.$message({
                  message: '请填写题目适用年龄！',
                  type: 'warning'
              })
              return false
            }

            if (!this.base.gender.length) {
              this.$message({
                  message: '请填写题目适用性别！',
                  type: 'warning'
              })
              return false
            }

            if (!this.base.education.length) {
              this.$message({
                  message: '请填写题目适用学历！',
                  type: 'warning'
              })
              return false
            }

            if (!this.base.content_type.length) {
              this.$message({
                  message: '请填写题目所属题材！',
                  type: 'warning'
              })
              return false
            }

            this.base.enterpriseCode = this.$route.query.enterpriseCode
            this.base.subjectCode = this.$route.query.subjectCode

            var formData = {
              enterpriseCode: this.$route.query.enterpriseCode,
              subjectCode: this.$route.query.subjectCode,
              subject_scenario: this.base.subject_scenario.join(','),
              age: this.base.age.join(','),
              content_type: this.base.content_type.join(','),
              consume_level: this.base.consume_level.join(','),
              education: this.base.education.join(','),
              gender: this.base.gender.join(','),
              shopping_guide: this.base.shopping_guide.join(','),
              career_type: this.base.career_type.join(','),
              unit_type: this.base.unit_type.join(','),
              industry_type: this.base.industry_type.join(','),
              content_politics: this.base.content_politics.join(',')
            }
            
            util.request({
                method: 'post',
                interface: 'createRelation',
                data: formData
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