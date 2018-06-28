<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <!-- <section class="block-title">时钟样式</section>
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
                    </section> -->

                    <section class="block-title">基本配置</section>
                    <section class="baseInput">
                        <span>壁纸</span>
                        <div class="input-box">
                            <upload :path="base.gameAnimateGif"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    :id-name="'gameAnimateGif'"
                                    @changeImg="changeImgTr"></upload>
                        </div>
                    </section>
                    <div class="clear"></div>

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
                    
                    <div class="clear"></div>
                    <section class="block-title">按钮样式</section>
                    <section class="baseInput">
                        <span>第一按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.playBtnOneFont">
                        </el-input>
                    </section>
                    <!-- <section class="baseInput rightF">
                        <span>第二按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.playBtnTwoFont">
                        </el-input>
                    </section> -->
                    <section class="baseInput rightF">
                        <span>第二按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.playBtnThreeFont">
                        </el-input>
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
            <section class="game-big-body" :style="gmPlayBox">
                <div class="clock-box"
                        v-if="base.clockStyle == '1'">
                        {{60 | formatDate}}
                </div>
                <section class="gmBodyArea gmPlay">
                    <div class="question-box">
                        <div class="question-title"
                            :style="questionTitle">
                            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. 
                        </div>
                    </div>
                    <section class="anwser-big-box">
                        <div class="anwser-box anwser-box-half"
                            :style="anwserStyle">

                            <div class="select-box"
                                    :style="selectBtnLineHeight">
                                <img src="../../../../assets/images/game-radio-icon.png">
                            </div>

                            <section class="anwser-option-box">
                                <div class="title-bg"></div>
                                <div class="question-title">
                                    第一个选项
                                </div>
                                <img class="img-anwser" src="/static/images/art1.jpg">
                            </section>
                        </div>
                        <div class="anwser-box anwser-box-half"
                            :style="anwserStyle">

                            <div class="select-box"
                                    :style="selectBtnLineHeight">
                                <img src="../../../../assets/images/game-radio-icon.png">
                            </div>

                            <section class="anwser-option-box">
                                <div class="title-bg"></div>
                                <div class="question-title">
                                    第二个选项
                                </div>
                                <img class="img-anwser" src="/static/images/art1.jpg">
                            </section>
                        </div>
                        <div class="anwser-box anwser-box-half"
                            :style="anwserStyle">

                            <div class="select-box"
                                    :style="selectBtnLineHeight">
                                <img src="../../../../assets/images/game-radio-icon.png">
                            </div>

                            <section class="anwser-option-box">
                                <div class="title-bg"></div>
                                <div class="question-title">
                                    第三个选项
                                </div>
                                <img class="img-anwser" src="/static/images/art1.jpg">
                            </section>
                        </div>
                        <div class="anwser-box anwser-box-half"
                            :style="anwserStyle">

                            <div class="select-box"
                                    :style="selectBtnLineHeight">
                                <img src="../../../../assets/images/game-radio-now.png">
                            </div>

                            <section class="anwser-option-box">
                                <div class="title-bg"></div>
                                <div class="question-title">
                                    第四个选项
                                </div>
                                <img class="img-anwser" src="/static/images/art1.jpg">
                            </section>
                        </div>
                    </section>
                </section>
            </section>
            <section class="play-btn-box">
                <div class="play-btn">
                    <img src="../../../../assets/images/cheat-icon.png">
                    {{base.playBtnOneFont}}
                </div>
                <!-- <span>|</span>
                <div class="play-btn">
                    <img src="../../../../assets/images/cheer-icon.png">
                    {{base.playBtnTwoFont}}
                </div> -->
                <span>|</span>
                <div class="play-btn">
                    <img src="../../../../assets/images/next-icon.png">
                    {{base.playBtnThreeFont}}
                </div>
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
                gameAnimateGif: '/static/images/game-bg.jpg',
                clockStyle: '1',
                playBtnFrameStyle: 'arrow',
                playQuestionMarginTop: '37',
                playQuestionLineHeight: '22',
                playQuestionFontSize: '16',
                playQuestionColor: '#2B2B41',
                playAnwserMarginTop: '13',
                playAnwserLineHeight: '22',
                playAnwserFontSize: '16',
                playAnwserColor: '#2B2B41',
                playImgMarginTop: '10',
                playImgHeight: '36',
                playAttachmentMarginTop: '6',
                playAttachmentHeight: '36',
                playAttachmentLineHeight: '24',
                playAttachmentFontSize: '16',
                playAttachmentColor: '#2B2B41',
                playBtnOneFont: '作弊',
                playBtnTwoFont: '助威',
                playBtnThreeFont: '下一题',
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
        formatDate (value) {
            var sec = value % 60
            var min = Math.floor(value / 60)
            min = min < 10 ? '0' + min : min
            sec = sec < 10 ? '0' + sec : sec

            return min + ' : ' + sec
        }
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
        },
        changeImgTr (data) {
            this.base.gameAnimateGif = data.url
        }
    },
    components: {
        upload
    }
}
</script>