<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">背景信息</section>
                    <section class="baseInput">
                        <span>擂主背景</span>
                        <div class="input-box">
                            <upload :path="base.pkPersonOneBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgOne"></upload>
                        </div>
                    </section>

                    <section class="block-title">按钮区域样式</section>
                    <section class="baseInput">
                        <span>第一按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.pkBtnOneFont">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>第二按钮文字</span>
                        <el-input
                                class="input-box"
                                v-model="base.pkBtnTwoFont">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮背景色</span>
                        <el-color-picker v-model="base.pkBtnBackColor"></el-color-picker>
                    </section>
                    <section class="baseInput">
                        <span>按钮文字颜色</span>
                        <el-color-picker v-model="base.pkBtnFontColor"></el-color-picker>
                    </section>
                    <section class="baseInput rightF">
                        <span>按钮边框颜色</span>
                        <el-color-picker v-model="base.pkBtnBorderColor"></el-color-picker>
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
            <div class="person-pk-box" :style="pkPersonBgStyle">
                <div class="person-box">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="title-box">
                        昵称
                    </div>
                    <div class="message-box">
                        15分
                    </div>
                </div>
                <div class="money-box">
                    押分10
                </div>
                <div class="person-box">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="title-box">
                        昵称
                    </div>
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
                <span :style="gmPkBtn">{{base.pkBtnOneFont}}</span>
                <span :style="gmPkBtn">{{base.pkBtnTwoFont}}</span>
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
                pkPersonOneBg: '/static/images/pk-bg.jpg',
                pkBtnOneFont: '应战',
                pkBtnTwoFont: '观战',
                pkBtnBackColor: '#2B2B41',
                pkBtnFontColor: '#ffffff',
                pkBtnBorderColor: '#2B2B41'
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
        changeImgOne (data) {
            this.base.pkPersonOneBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>