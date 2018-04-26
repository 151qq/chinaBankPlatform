<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">个人信息</section>
                    <section class="baseInput">
                        <span>个人信息背景</span>
                        <div class="input-box">
                            <upload :path="base.helpPersonBg"
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
                                v-model="base.helpBtnFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.helpBtnBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.helpBtnFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.helpBtnBorderColor"></el-color-picker>
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
            <div class="person-message-box" :style="helpPersonBgStyle">
                <div class="user-info-box">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="user-content">
                        <span class="user-name">昵称</span>
                        <span class="user-text">大家帮帮我呀</span>
                    </div>
                    <div class="user-info">
                        <img src="../../../../assets/images/ranking-icon.png">
                        <div class="user-num">27名</div>
                        <div>/共22333名</div>
                    </div>
                </div>
                <div class="user-data-box">
                    90%
                </div>
            </div>

            <section class="gmBodyArea">
                <section class="game-comment-box">
                    <section class="comment-b">
                        <div class="avatar-box">
                            <img src="/static/images/art1.jpg">
                        </div>
                        <div class="content-box">
                            <div class="title-box">
                                <span class="title">昵称</span>
                            </div>
                            <div class="des-box">
                                昵称赠送500积分
                            </div>
                        </div>
                    </section>
                    <section class="comment-b">
                        <div class="avatar-box">
                            <img src="/static/images/art1.jpg">
                        </div>
                        <div class="content-box">
                            <div class="title-box">
                                <span class="title">昵称</span>
                            </div>
                            <div class="des-box">
                                昵称赠送500积分
                            </div>
                        </div>
                    </section>
                </section>
            </section>
            <section class="bottom-btn-box">
                <span :style="gmHelpBtn">{{base.helpBtnFont}}</span>
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
                helpPersonBg: '/static/images/person-bg.jpg',
                helpBtnFont: '我来助威',
                helpBtnBackColor: '#2B2B41',
                helpBtnFontColor: '#ffffff',
                helpBtnBorderColor: '#2B2B41'
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
            this.base.helpPersonBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>