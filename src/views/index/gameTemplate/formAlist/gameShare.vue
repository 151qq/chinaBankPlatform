<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">背景样式</section>
                    <section class="baseInput">
                        <span>分享背景</span>
                        <div class="input-box">
                            <upload :path="base.shareBigBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgBg"></upload>
                        </div>
                    </section>

                    <div class="clear"></div>
                    <!-- 大标题样式 -->
                    <section class="block-title">内容样式</section>
                    <section class="baseInput">
                        <span>头像大小</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.shareAttarHeight">
                        </el-input>
                    </section>
                    <section class="baseInput rightF">
                        <span>头像偏移</span>
                        <el-input
                                class="input-box"
                                type="number"
                                v-model="base.shareAttarMarginTop">
                        </el-input>
                    </section>
                    <section class="baseInput">
                        <span>称号背景</span>
                        <div class="input-box">
                            <upload :path="base.shareNameBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgName"></upload>
                        </div>
                    </section>
                    <section class="baseInput rightF">
                        <span>称号字体高度</span>
                        <el-input
                                class="input-box"
                                type="number"
                                :min="0"
                                v-model="base.shareNameLineHeight">
                        </el-input>
                    </section>

                    <!-- <section class="block-title">按钮区域样式</section>
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
                    </section> -->
                    <div class="clear"></div>
                </div>
                <el-button v-if="isEdit"
                            class="save-btn" type="info" :plain="true" size="small" icon="document"
                           @click="saveBase">保存</el-button>
                <div class="clear"></div>
            </div>
        </section>
        <section class="right-content" :style="shareBgBox">
            <section class="gmBodyArea">
                <div class="share-box">
                    <div class="share-title">
                        <img src="/static/images/share-title.png">
                    </div>
                    <div class="person-box">
                        <div class="attar-box">
                            <img class="bg-attar" src="/static/images/img-out.png">
                            <img :style="shareAttarStyle" class="attar" src="/static/images/art1.jpg">
                        </div>
                        <div class="person">
                            <div class="name-box">王小明</div>
                            <div class="game-name" :style="shareNameBg">
                                理财天才
                            </div>
                            <div class="user-num">
                                <img src="../../../../assets/images/ranking-icon.png">
                                <div class="num-box">27名</div>
                                <div>/共12008名</div>
                            </div>
                        </div>
                    </div>

                    <div class="ewm-box">
                        <img src="/static/images/ewm.png">
                        <span>长按二维码进入游戏</span>
                    </div>
                </div>
            </section>
            <!-- <section class="bottom-btn-box">
                <span :style="gmShareBtn">{{base.shareBtnFont}}</span>
            </section> -->
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
                shareBigBg: '/static/images/share-bg.jpg',
                shareNameBg: '/static/images/name-bg.png',
                shareNameLineHeight: '43',
                shareAttarHeight: '75',
                shareAttarMarginTop: '0'
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
        changeImgBg (data) {
            this.base.shareBigBg = data.url
        },
        changeImgName (data) {
            this.base.shareAttarBg = data.url
        }
    },
    components: {
        upload
    }
}
</script>