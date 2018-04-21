<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">标题样式</section>
                    <section class="baseInput">
                        <span>区域高度</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.titleBlockHeight">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>最大字数</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.titleMaxInput">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>字体大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.titleFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>行间距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.titleLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>字体颜色</span>
                        <el-color-picker v-model="base.titleFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>字体样式</span>
                        <el-select class="input-box"
                                   v-model="base.titleFontStyle"
                                   name="investor"
                                   placeholder="请选择">
                            <el-option
                                    v-for="(item, index) in fontStyles"
                                    :key="index"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </section>
                    <section class="baseInput">
                        <span>对齐方式</span>
                        <el-select class="input-box"
                                   v-model="base.titleAlignment"
                                   name="investor"
                                   placeholder="请选择">
                            <el-option
                                    v-for="(item, index) in alignmentStyles"
                                    :key="index"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </section>
                    <section class="baseInput rightF">
                        <span>顶部空隙</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.titleTopHeight">
                        </el-input>
                    </section>

                    <!-- 文本区域样式 -->
                    <section class="block-title">文本区域样式</section>
                    <section class="baseInput rightF">
                        <span>最大字数</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.fileTaxtMaxInput">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>字体大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.fileFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>行间距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.fileTaxtLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>字体颜色</span>
                        <el-color-picker v-model="base.fileTaxtFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>顶部空隙</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.fileBlcokTopHeight">
                        </el-input>
                    </section>
                    <div class="clear"></div>
                    <!-- 按钮区域样式 -->
                    <section class="block-title">按钮区域样式</section>
                    <section class="baseInput">
                        <span>按钮排列样式</span>
                        <el-select class="input-box"
                                   v-model="base.btnFrameStyle"
                                   name="investor"
                                   placeholder="请选择">
                            <el-option
                                    v-for="(item, index) in btnFrameStyles"
                                    :key="index"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </section>
                    <section class="baseInput rightF">
                        <span>第一按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.startBtnOneFont">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>第二按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.startBtnTwoFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.startBtnBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.startBtnFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.startBtnBorderColor"></el-color-picker>
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
            <div class="ar-title" :style="gmTitle">
                标题，可以居中也可以居左，最多两行
            </div>
            <section class="ar-text-body" :style="gmContent">
                <div style="text-align: center;">
                    葡萄美酒夜光杯，欲饮琵琶马上催。 <br>
                    醉卧沙场君莫笑，古来征战几人回？
                </div>
            </section>
            <section class="start-btn-box" :style="gmStartBtnBox">
                <span :style="gmStartBtn">{{base.startBtnOneFont}}</span>
                <span :style="gmStartBtn">{{base.startBtnTwoFont}}</span>
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
                titleBlockHeight: 76,
                titleMaxInput: 30,
                titleFontSize: 18,
                titleLineHeight: 30,
                titleFontColor: '#000000',
                titleFontStyle: 'normal',
                titleAlignment: 'center',
                titleTopHeight: 3,

                fileTaxtMaxInput: 500,
                fileFontSize: 14,
                fileTaxtLineHeight: 24,
                fileFontType: 'normal',
                fileBlcokTopHeight: 0,
                fileBackground: '',

                btnFrameStyle: 'row',
                startBtnOneFont: '第一按钮',
                startBtnTwoFont: '第二按钮',
                startBtnBackColor: '#50D76D',
                startBtnFontColor: '#ffffff',
                startBtnBorderColor: '#50D76D'
            },
            templateTypes: [],
            fontStyles: [
                {
                    label: '标准',
                    value: 'normal'
                },
                {
                    label: '斜体',
                    value: 'italic'
                },
                {
                    label: '倾斜',
                    value: 'oblique'
                }
            ],
            alignmentStyles: [
                {
                    label: '左对齐',
                    value: 'left'
                },
                {
                    label: '居中',
                    value: 'center'
                },
                {
                    label: '右对齐',
                    value: 'right'
                },
                {
                    label: '两端对齐',
                    value: 'justify'
                }
            ],
            btnFrameStyles: [
                {
                    label: '左右排列',
                    value: 'row'
                },
                {
                    label: '上下排列',
                    value: 'column'
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