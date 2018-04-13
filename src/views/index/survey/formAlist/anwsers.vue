<template>
    <section class="anwsers-box">
        <el-button class="add-btn" type="primary" icon="plus" size="small" @click="addItem">增加</el-button>
        <div class="content-body-box" v-for="(item, index) in anwsersList">
            <div class="select-box">
                <div class="single-box">
                    <img src="../../../../assets/images/radio-now.png">
                </div>

                <div class="check-box">
                    <img src="../../../../assets/images/check-now.png">
                </div>
            </div>
            <section>
                <div>
                    {{item.subjectContent}}
                </div>
                <div>
                    <small-imgs :attachment-data="item.attachment" :is-operate="true"
                        :attachment-type="'subjectQustion'"></small-imgs>
                </div>
                <div>
                    <article-list :article-list="item.articleList" @deleteArticle="deleteArticle"></article-list>
                </div>
                <div class="btns-box">
                    <el-button @click="deleteItem(item)" type="primary" size="mini">删除</el-button>
                    <el-button @click="editItem(item)" type="primary" size="mini">编辑</el-button>
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
            <el-form-item label="选项">
                <el-input
                    type="textarea"
                    :rows="3"
                    placeholder="请输入内容"
                    v-model="addFormData.subjectContent">
                </el-input>
            </el-form-item>
            <el-form-item label="选项图片">
                <small-imgs :attachment-data="addFormData.attachment" :is-operate="true"
                        :attachment-type="'subjectQustion'"
                        :is-popup="true"
                        @imgClick="imgClick"></small-imgs>
            </el-form-item>
            <el-form-item>
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

export default {
    data () {
        return {
            anwsersList: [],
            articleCode: [],
            articleList: [],
            isOperate: true,
            isAddItem: false,
            addFormData: {
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
        // this.getQuestions()
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
        deleteItem (item) {
            util.request({
                method: 'post',
                interface: 'deleteSurveySubject',
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
            this.addFormData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                surveyCode: this.$route.query.surveyCode,
                subjectContent: '',
                subjectSequence: 0,
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
            util.request({
                method: 'post',
                interface: 'manageSurveySubject',
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

    }

    .btn-show {
        margin-bottom: 10px;

        .btn-hover {
            display: block;
            margin-top: 10px;
            overflow: hidden;

            .sort-box {
                float: left;

                .sortInput {
                    width: 80px;
                    height: 28px;

                    .el-input__inner {
                        height: 28px;
                    }

                    .el-input-group__append {
                        padding: 0;
                    }
                }
            }
        }

        .btns {
            background: #EFF2F7;
            display: block;
            padding: 12px;
            overflow: hidden;

            .del-btn {
              float: right;
              width: 16px;
              height: 16px;
              cursor: pointer;
              margin-left: 9px;

              &:hover {
                opacity: 0.8;
              }
            }
        }


        .delete-btn {
            float: right;
            margin-left: 10px;
        }
    }
}
</style>