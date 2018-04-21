<template>
    <section class="anwsers-box">
        <el-button class="add-btn" type="primary" icon="plus" size="small" @click="addItem">增加</el-button>
        <div class="content-body-box" v-for="(item, index) in anwsersList">
            <div class="select-box" @click="selectAnwser(item)">
                <div class="single-box" v-if="item.subjectChooseType == '1'">
                    <img v-if="item.subjectAnswer == '1'" src="../../../../assets/images/radio-now.png">
                    <img v-else src="../../../../assets/images/radio-icon.png">
                </div>

                <div class="check-box" v-if="item.subjectChooseType == '0'">
                    <img v-if="item.subjectAnswer == '1'" src="../../../../assets/images/check-now.png">
                    <img v-else src="../../../../assets/images/check-icon.png">
                </div>
            </div>
            <section class="anwser-content">
                <div class="anwser-title content-box" @click="selectAnwser(item)">
                    {{item.subjectContent}}
                </div>
                <div class="content-box" v-if="item.attachment.length">
                    <small-imgs :attachment-data="item.attachment"
                        :attachment-type="'subjectQustion'"></small-imgs>
                </div>
                <div class="content-box" v-if="item.articleList.length">
                    <article-list :article-list="item.articleList"></article-list>
                </div>
                <div class="btns-box">
                    <el-button class="btn-list" @click="deleteItem(item)" :plain="true" type="danger" size="mini">删除</el-button>
                    <el-button class="btn-list" @click="editItem(item)" :plain="true" type="info" size="mini">编辑</el-button>
                </div>
            </section>
        </div>

        <div v-if="!anwsersList.length" class="null-page">
            暂无选项
        </div>

        <el-dialog title="添加选项" :visible.sync="isAddItem" class="anwser-box-add">
          <el-form :label-position="'left'" :model="addFormData" label-width="80px">
            <el-form-item label="序号">
                <el-input
                    type="number"
                    :min="0"
                    placeholder="请输入"
                    v-model="addFormData.subjectSequence">
                </el-input>
            </el-form-item>

            <el-form-item label="选项" v-if="base.optionCssType == '2'">
                <el-input
                    type="textarea"
                    :rows="1"
                    :max="9"
                    placeholder="请输入内容,最多9个字"
                    v-model="addFormData.subjectContent">
                </el-input>
            </el-form-item>

            <el-form-item label="选项" v-else>
                <el-input
                    type="textarea"
                    :rows="3"
                    :max="140"
                    placeholder="请输入内容,最多140个字"
                    v-model="addFormData.subjectContent">
                </el-input>
            </el-form-item>

            <el-form-item label="选项图片" v-if="base.optionCssType != '1'">
                <small-imgs :attachment-data="addFormData.attachment"
                            :is-operate="true"
                            :attachment-type="'subjectQustion'"
                            :is-popup="true"
                            :img-num="base.optionCssType == '2' ? 1 : 9"
                            @imgClick="imgClick"></small-imgs>
            </el-form-item>
        
            <el-form-item v-if="base.optionCssType == '1'">
                <div class="btn-color" @click="addAttachment" slot="label">添加附件</div>
                <article-list :article-list="addFormData.articleList"
                                @deleteArticle="deleteArticle"
                                :is-edit="true"></article-list>
            </el-form-item>

          </el-form>
          <div slot="footer" class="dialog-footer">
                <el-button @click="isAddItem = false">取 消</el-button>
                <el-button type="primary" @click="confirmItem">确 定</el-button>
          </div>
        </el-dialog>

        <popup-load  :is-operate="true"
                     :bg-path="true"
                     :id-name="'catalogImage'"
                     :is-upload="isUpload"
                     @changeImg="changeItemImg"></popup-load>

        <attachment-article :visible-data="visibleData"
                            :article-code="addFormData.formData.attachmentSourceCodes"
                          :articleList="addFormData.articleList"
                          @getAttachments="setAttachments"
                          :ref="'anwser'"></attachment-article>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import smallImgs from '../../../../components/common/small-imgs'
import popupLoad from '../../../../components/common/popupLoad.vue'
import attachmentArticle from '../../../../components/common/attachment-article'
import articleList from './articleList'
import { mapGetters } from 'vuex'

export default {
    props: ['base'],
    data () {
        return {
            anwsersList: [],
            articleCode: [],
            articleList: [],
            isOperate: true,
            isAddItem: false,
            addFormData: {
                subjectType: '0',
                subjectParent: '',
                subjectContent: '',
                subjectSequence: 0,
                attachment: [],
                formData: {
                    attachmentTargetType: 'subjectQustion',
                    attachmentSourceType: "attachmen_type_4",
                    attachmentSourceCodes: []
                },
                articleList: []
            },
            isUpload: {
                value: false
            },
            visibleData: {
              show: false
            }
        }
    },
    mounted () {
        this.getQuestions()
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
        imgClick () {
            this.isUpload.value = true
        },
        changeItemImg (data) {
            var imgData = {
              attachmentTargetType: 'subjectAnwsers',
              attachmentSourceType: "attachmen_type_8",
              attachmentSourceCode: data.url
            }

            if (data.type == 'local') {
                imgData.attachmentSourceType = 'attachmen_type_1'
            }

            this.addFormData.attachment.push(imgData)
        },
        getQuestions () {
            util.request({
                method: 'get',
                interface: 'optiondetails',
                data: {
                    subjectCode: this.$route.query.subjectCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.anwsersList = res.result.result
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        selectAnwser (item) {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                subjectParent: this.$route.query.subjectCode,
                subjectCode: item.subjectCode,
                subjectChooseType: item.subjectChooseType,
                subjectAnswer: item.subjectAnswer ? '0' : '1'
            }

            if (item.subjectChooseType == '1') {
                formData.subjectAnswer = '1'
            }

            util.request({
                method: 'post',
                interface: 'changeAnwser',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                    this.getQuestions()
                    this.$message({
                        type: 'success',
                        message: '修改成功!'
                    })
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        deleteItem (item) {
            util.request({
                method: 'get',
                interface: 'deleteSubject',
                data: {
                  subjectCode: item.subjectCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.getQuestions()
                    this.$message({
                        type: 'success',
                        message: '删除成功!'
                    })
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        addItem () {
            if (this.base.optionCssType == '2' && this.anwsersList.length >= 4) {
                this.$message({
                    message: '最多添加4个选项！',
                    type: 'warning'
                })
                return false
            }

            this.addFormData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                subjectParent: this.$route.query.subjectCode,
                optionCssType: this.base.optionCssType,
                subjectCssType: this.base.subjectCssType,
                subjectChooseType: this.base.subjectChooseType,
                subjectAnwser: '0',
                subjectType: '0',
                subjectContent: '',
                subjectSequence: '',
                attachment: [],
                formData: {
                    attachmentTargetType: 'subjectQustion',
                    attachmentSourceType: "attachmen_type_4",
                    attachmentSourceCodes: []
                },
                articleList: []
            }

            this.isAddItem = true
        },
        editItem (item) {
            this.addFormData = Object.assign({}, item)

            this.isAddItem = true
        },
        confirmItem () {
            if (!this.addFormData.subjectSequence) {
                this.$message({
                    message: '请填写选项序号！',
                    type: 'warning'
                })
                return false
            }

            if (!this.addFormData.subjectContent) {
                this.$message({
                    message: '请填写选项内容！',
                    type: 'warning'
                })
                return false
            }

            util.request({
                method: 'post',
                interface: 'manageSubjectInfo',
                data: this.addFormData
            }).then(res => {
                if (res.result.success == '1') {
                    this.getQuestions()
                    this.isAddItem = false
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        addAttachment (index) {
            var anwserData = this.addFormData

            this.articleCode = [].concat(anwserData.formData.attachmentSourceCodes)
            this.articleList = [].concat(anwserData.articleList)
            this.visibleData.show = true
            this.$refs['anwser'].initData()
        },
        setAttachments (articleData) {
            var anwserData = this.addFormData
            anwserData.formData.attachmentSourceCodes = [].concat(articleData.codes)
            anwserData.articleList = [].concat(articleData.datas)
        },
        deleteArticle (index) {
            var anwserData = this.addFormData

            anwserData.articleList.splice(index, 1)
            anwserData.formData.attachmentSourceCodes.splice(index, 1)
        }
    },
    components: {
        smallImgs,
        attachmentArticle,
        articleList,
        popupLoad
    }
}
</script>
<style lang="scss">
.anwsers-box {
    .anwser-box-add {
        .el-dialog  {
            width: 800px;
        }
    }

    .content-body-box {
        display: flex;

        .select-box {
            padding-top: 4px;
            margin-right: 10px;
            cursor: pointer;

            img {
                display: block;
                width: 16px;
                height: 16px;
            }
        }

        .anwser-content {
            flex: 1;

            .anwser-title {
                font-size: 16px;
                line-height: 24px;
                color: #000000;
                cursor: pointer;
            }

            .content-box {
                margin-bottom: 10px;
            }
        }

        .btns-box {
            text-align: right;

            .btn-list {
                margin-left: 10px;
            }
        }
    }
}
</style>