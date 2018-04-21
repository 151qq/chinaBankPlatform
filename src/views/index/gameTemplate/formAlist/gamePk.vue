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
                    <section class="baseInput rightF">
                        <span>挑战者背景</span>
                        <div class="input-box">
                            <upload :path="base.pkPersonTwoBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgTwo"></upload>
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
            <div class="person-pk-box">
                <div class="person-box" :style="pkPersonOneBgStyle">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="message-box">
                        昵称<br>
                        15分
                    </div>
                </div>
                <div class="person-box" :style="pkPersonTwoBgStyle">
                    <div class="attar-box">
                        <img src="/static/images/art1.jpg">
                    </div>
                    <div class="message-box">
                        昵称<br>
                        15分
                    </div>
                </div>
            </div>

            <section class="gmBodyArea">
                <div class="money-box">
                    押分10
                </div>

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
                pkPersonOneBg: '/static/images/art1.jpg',
                pkPersonTwoBg: '/static/images/art1.jpg',
                pkBtnOneFont: '应战',
                pkBtnTwoFont: '观战',
                pkBtnBackColor: '#50D76D',
                pkBtnFontColor: '#ffffff',
                pkBtnBorderColor: '#50D76D'
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
        },
        changeImgTwo (data) {
            this.base.pkPersonTwoBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>