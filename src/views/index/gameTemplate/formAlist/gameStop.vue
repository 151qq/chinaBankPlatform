<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">个人信息</section>
                    <section class="baseInput">
                        <span>个人信息行距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.stopPersonLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>个人信息字号</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.stopPersonFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>个人信息背景</span>
                        <div class="input-box">
                            <upload :path="base.stopPersonBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImg"></upload>
                        </div>
                    </section>
                    <section class="baseInput rightF">
                        <span>个人信息字颜色</span>
                        <el-color-picker v-model="base.stopPersonColor"></el-color-picker>
                    </section>

                    <section class="block-title">按钮区域样式</section>
                    <section class="baseInput">
                        <span>按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.stopBtnOneFont">
                        </el-input>
                    </section>
                    <!-- <section class="baseInput rightF">
                        <span>第二按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.stopBtnTwoFont">
                        </el-input>
                    </section> -->
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.stopBtnBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.stopBtnFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.stopBtnBorderColor"></el-color-picker>
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
            <div class="person-message-box" :style="stopPersonBgStyle">
                <div class="person-box">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div :style="personStyle">
                        昵称<br>
                        XXX大师<br>
                        第27名（120890）<br>
                    </div>
                </div>
                <div :style="stopFontStyle">
                    {{base.stopSuccessText}}|{{base.stopFailText}}
                </div>
            </div>

            <section class="gmBodyArea">
                <div class="money-box">
                    您新增积分N分/<span>总积分123450</span>
                    <p>分享战绩可以获得更多积分</p>
                </div>

                <a class="honor-box">
                    <span class="money-num">¥ 20</span>
                    <span class="btn-font">
                        <img src="../../../../assets/images/ticket-icon.png">
                        领取奖励
                    </span>
                </a>
            </section>
            
            <section class="bottom-btn-box">
                <span :style="gmStopBtn">{{base.stopBtnOneFont}}</span>
                <span :style="gmStopBtn">{{base.stopBtnTwoFont}}</span>
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
                stopFailText: '加油',
                stopSuccessText: '恭喜',
                stopPersonBg: '/static/images/person-bg.jpg',
                stopPersonColor: '#ffffff',
                stopFontSize: '16',
                stopPersonLineHeight: '24',
                stopPersonFontSize: '14',
                stopFontColor: '#ffffff',
                stopBtnOneFont: '继续闯关',
                stopBtnTwoFont: '见好就收',
                stopBtnBackColor: '#2B2B41',
                stopBtnFontColor: '#ffffff',
                stopBtnBorderColor: '#2B2B41',
            }
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
        changeImg (data) {
            this.base.stopPersonBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>