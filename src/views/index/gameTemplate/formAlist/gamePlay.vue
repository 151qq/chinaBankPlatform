<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">时钟样式</section>
                    <section class="baseInput">
                        <span>时钟样式</span>
                        <el-select class="input-box"
                                   v-model="base.clockStyle"
                                   name="investor"
                                   placeholder="请选择">
                            <el-option
                                    v-for="(item, index) in clockStyles"
                                    :key="index"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </section>

                    <section class="block-title">题目样式</section>
                    <section class="baseInput">
                        <span>题目顶部距离</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playQuestionMarginTop">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>题目行距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playQuestionLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>题目文字大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playQuestionFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>题目文字颜色</span>
                        <el-color-picker v-model="base.playQuestionColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>选项顶部高度</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAnwserMarginTop">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>选项行距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAnwserLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>选项文字大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAnwserFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>选项文字颜色</span>
                        <el-color-picker v-model="base.playAnwserColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>图片顶部距离</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playImgMarginTop">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>图片高度</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playImgHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>附件顶部距离</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAttachmentMarginTop">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>附件区域高度</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAttachmentHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>附件行距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAttachmentLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>附件字体大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.playAttachmentFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>附件字体颜色</span>
                        <el-color-picker v-model="base.playAttachmentColor"></el-color-picker>
                    </section>
                    

                    <section class="block-title">按钮样式</section>
                    <section class="baseInput">
                        <span>底部按钮选择</span>
                        <el-select class="input-box"
                                   v-model="base.playBtnFrameStyle"
                                   name="investor"
                                   placeholder="请选择">
                            <el-option
                                    v-for="(item, index) in playBtnFrameStyles"
                                    :key="index"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </section>
                    <div class="clear"></div>
                </div>
                <el-button v-if="isEdit"
                            class="save-btn" type="info" :plain="true" size="small" icon="document"
                           @click="saveBase">保存</el-button>
                <div class="clear"></div>
            </div>
        </section>
        <section class="right-content">
            <div class="clock-box">{{base.clockStyle}}</div>
            <section class="gmBodyArea">
                <div class="question-box">
                    <div class="question-title" :style="questionTitle">
                        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. 
                    </div>
                    <div class="question-imgs-box" :style="questionImg">
                        <img :style="questionImgStyle" src="/static/images/art1.jpg">
                    </div>
                    <div class="question-attachment-box">
                        <div class="attachment-box" :style="questionAttachmentStyle">
                            <div class="img-box">
                                <img src="/static/images/art1.jpg">
                            </div>
                            <div class="attachment-content">
                                <div class="attachment-title" :style="attachmentFontStyle">
                                    附件标题附件标题附件标题附件标题
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="anwser-box" :style="anwserStyle">
                    <div class="select-box" :style="selectBtnLineHeight">
                        <img src="../../../../assets/images/radio-icon.png">
                    </div>

                    <section>
                        <div class="question-title" :style="anwserTitle">
                            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. 
                        </div>
                        <div class="question-imgs-box" :style="questionImg">
                            <img :style="questionImgStyle" src="/static/images/art1.jpg">
                        </div>
                        <div class="question-attachment-box">
                            <div class="attachment-box" :style="questionAttachmentStyle">
                                <div class="img-box">
                                    <img src="/static/images/art1.jpg">
                                </div>
                                <div class="attachment-content"  :style="attachmentFontStyle">
                                    附件标题附件标题附件标题附件标题
                                </div>
                            </div>
                        </div>
                    </section>
                </div>
            </section>
            <section class="bottom-btn-box">
                <i v-if="base.playBtnFrameStyle == 'arrow'" class="el-icon-d-arrow-left btn-box"></i>
                <el-button class="btn-box" type="info">作弊</el-button>
                <el-button class="btn-box" type="info">求援</el-button>
                <i v-if="base.playBtnFrameStyle == 'arrow'" class="el-icon-d-arrow-right btn-box"></i>
            </section>
        </section>
    </section>
</template>
<script>
import upload from '../../../../components/common/upload'
import util from '../../../../assets/common/util'
import templateMixin from '../../../../assets/common/gameTemplateMix'
import { mapGetters } from 'vuex'
export default {
    props: ['baseData', 'fileType'],
    data () {
        return {
            isOperate: true,
            base: {
                clockStyle: '1',
                playBtnFrameStyle: 'arrow',
                playQuestionMarginTop: '10',
                playQuestionLineHeight: '24',
                playQuestionFontSize: '14',
                playQuestionColor: '#000000',
                playAnwserMarginTop: '10',
                playAnwserLineHeight: '20',
                playAnwserFontSize: '14',
                playAnwserColor: '#000000',
                playImgMarginTop: '10',
                playImgHeight: '50',
                playAttachmentMarginTop: '10',
                playAttachmentHeight: '50',
                playAttachmentLineHeight: '24',
                playAttachmentFontSize: '14',
                playAttachmentColor: '#000000'
            },
            templateTypes: [],
            clockStyles: [
                {
                    label: '标准',
                    value: '1'
                },
                {
                    label: '炸弹',
                    value: '2'
                },
                {
                    label: '时钟',
                    value: '3'
                }
            ],
            playBtnFrameStyles: [
                {
                    label: '带箭头',
                    value: 'arrow'
                },
                {
                    label: '不带箭头',
                    value: 'noarrow'
                }
            ]
        }
    },
    mixins: [templateMixin],
    filters: {
        formatDate: util.formatDate
    },
    mounted () {
        this.base = Object.assign(this.base, this.baseData)
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
        saveBase () {
            util.request({
                method: 'post',
                interface: 'gamesubjecttemplateCreate',
                data: this.base
            }).then(res => {
                if (res.result.success == '1') {
                    this.$emit('getBase')
                    this.$message({
                        message: '保存模版成功！',
                        type: 'success'
                    })
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