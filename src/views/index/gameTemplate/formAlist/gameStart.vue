<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <section class="block-title">基本配置</section>
                    <section class="baseInput">
                        <span>游戏壁纸</span>
                        <div class="input-box">
                            <upload :path="base.gameBigBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    :id-name="'gameBigBg'"
                                    @changeImg="changeImgBg"></upload>
                        </div>
                    </section>
                    <section class="baseInput rightF">
                        <span>过场动画</span>
                        <div class="input-box">
                            <upload :path="base.gameAnimateGif"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    :id-name="'gameAnimateGif'"
                                    @changeImg="changeImgTr"></upload>
                        </div>
                    </section>
                    
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
                    <!-- <section class="block-title">第一按钮区域</section>
                    <section class="baseInput">
                        <span>按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.startBtnOneFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.startBtnOneBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.startBtnOneFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.startBtnOneBorderColor"></el-color-picker>
                    </section>
                    <div class="clear"></div> -->
                    <!-- 按钮区域样式 -->
                    <section class="block-title">按钮区域</section>
                    <section class="baseInput">
                        <span>按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.startBtnTwoFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.startBtnTwoBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.startBtnTwoFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.startBtnTwoBorderColor"></el-color-picker>
                    </section>
                    <div class="clear"></div>
                </div>
                <el-button v-if="isEdit"
                            class="save-btn" type="info" :plain="true" size="small" icon="document"
                           @click="saveBase">保存</el-button>
                <div class="clear"></div>
            </div>
        </section>
        <section class="right-content" :style="gmBgBox">
            <div class="ar-title" :style="gmTitle">
                达人挑战赛<br>
                百万赢家就是你
            </div>
            <div class="game-desc-box">游戏说明</div>
            <section class="ar-text-body" :style="gmContent">
                消耗十积分后开进行比赛，进入比赛并取得比赛结果，根据最终的得分，赢得比赛奖金。<br>
                答题越多月准确者得分越高，未答题或者答错不得分，但仍可答下一题。消耗十积分后开进行比赛，进入比赛并取得比赛结果。<br>
                根据最终的得分，赢得比赛奖金。
            </section>
            <section class="start-btn-box">
                <!-- <span :style="gmStartBtnOne">
                    {{base.startBtnOneFont}}
                </span> -->
                <span :style="gmStartBtnTwo">
                    {{base.startBtnTwoFont}}
                </span>
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
                gameBigBg: '/static/images/game-bg.jpg',
                gameAnimateGif: '',
                gamePkAnimateGif: '',
                titleBlockHeight: 88,
                titleMaxInput: 30,
                titleFontSize: 30,
                titleLineHeight: 44,
                titleFontColor: '#000000',
                titleFontStyle: 'normal',
                titleAlignment: 'left',
                titleTopHeight: 26,

                fileTaxtMaxInput: 500,
                fileFontSize: 12,
                fileTaxtLineHeight: 24,
                fileFontType: 'normal',
                fileBlcokTopHeight: 12,
                fileTaxtFontColor: '#7D7C81',

                startBtnOneFont: '找朋友PK',
                startBtnTwoFont: '自我挑战',
                startBtnOneBackColor: '#A6AFC4',
                startBtnOneFontColor: '#ffffff',
                startBtnOneBorderColor: '#A6AFC4',

                startBtnTwoBackColor: '#2B2B41',
                startBtnTwoFontColor: '#ffffff',
                startBtnTwoBorderColor: '#2B2B41'
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
        },
        changeImgBg (data) {
            this.base.gameBigBg = data.url
            this.imgShowUrl = data.url
        },
        changeImgTr (data) {
            this.base.gameAnimateGif = data.url
            this.imgShowUrl = data.url
        }
    },
    components: {
        upload
    }
}
</script>