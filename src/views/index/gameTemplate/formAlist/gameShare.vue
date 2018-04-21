<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">个人信息</section>
                    <section class="baseInput">
                        <span>感谢支持</span>
                        <el-input
                                class="input-box"
                                v-model="base.shareThanksText">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>感谢支持字号</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.shareThanksFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>感谢支持颜色</span>
                        <el-color-picker v-model="base.shareThanksColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>个人信息行距</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.sharePersonLineHeight">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>个人信息字号</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.sharePersonFontSize">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>个人信息字颜色</span>
                        <el-color-picker v-model="base.sharePersonColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>个人信息背景</span>
                        <div class="input-box">
                            <upload :path="base.sharePersonBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImg"></upload>
                        </div>
                    </section>

                    <section class="block-title">按钮区域样式</section>
                    <section class="baseInput">
                        <span>按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.shareBtnFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.shareBtnBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.shareBtnFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.shareBtnBorderColor"></el-color-picker>
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
            <div class="person-message-box" :style="sharePersonBgStyle">
                <div class="person-box">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div :style="sharePersonStyle">
                        昵称<br>
                        XXX大师<br>
                        第27名（120890）<br>
                    </div>
                </div>
                <div :style="shareFontStyle">
                    {{base.shareThanksText}}
                </div>
            </div>

            <section class="gmBodyArea">
                <div class="comment-box">
                    <div class="comment-attar">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="comment-content">
                        <span class="comment-name">昵称赠送500积分</span>
                        <span class="comment-text">加油</span>

                        <div class="comment-imgs">
                            <img src="/static/images/art1.jpg">
                            <img src="/static/images/art1.jpg">
                        </div>
                    </div>
                </div>

                <div class="comment-box">
                    <div class="comment-attar">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="comment-content">
                        <span class="comment-name">昵称</span>
                        <span class="comment-text">加油加油加油加油加油</span>

                        <div class="comment-imgs">
                            <img src="/static/images/art1.jpg">
                        </div>
                    </div>
                </div>
            </section>
            <section class="bottom-btn-box">
                <span :style="gmShareBtn">{{base.shareBtnFont}}</span>
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
                shareThanksText: '感谢支持',
                shareThanksFontSize: '14',
                shareThanksColor: '#ffffff',
                sharePersonLineHeight: '24',
                sharePersonFontSize: '14',
                sharePersonColor: '#ffffff',
                sharePersonBg: '/static/images/art1.jpg',
                shareBtnFont: '我来挑战',
                shareBtnBackColor: '#50D76D',
                shareBtnFontColor: '#ffffff',
                shareBtnBorderColor: '#50D76D'
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
            this.base.sharePersonBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>