<template>
    <section class="article-template-box">
        <section class="left-content form-b">
            <div class="formStylePro">
                <div class="form-box">
                    <div class="clear"></div>
                    <section class="baseInput">
                        <span>游戏壁纸</span>
                        <div class="input-box" @mouseover="showImg('gameBigBg')">
                            <upload :path="base.gameBigBg"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgBg"></upload>
                        </div>
                    </section>
                    <section class="baseInput rightF">
                        <span>过场动画</span>
                        <div class="input-box" @mouseover="showImg('gameAnimateGif')">
                            <upload :path="base.gameAnimateGif"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgTr"></upload>
                        </div>
                    </section>
                    <div class="clear"></div>
                    <section class="baseInput">
                        <span>PK结果动画</span>
                        <div class="input-box" @mouseover="showImg('gamePkAnimateGif')">
                            <upload :path="base.gamePkAnimateGif"
                                    :is-operate="isEdit"
                                    :bg-path="false"
                                    @changeImg="changeImgEn"></upload>
                        </div>
                    </section>
                    <div class="clear"></div>
                </div>
                <el-button v-if="isEdit"
                            class="save-btn" type="info" :plain="true" size="small" icon="document"
                           @click="saveBase">保存</el-button>
                <div class="clear"></div>
            </div>
        </section>
        <section class="right-content" v-if="imgShowUrl">
            <img :style="pcImgStyle" :src="imgShowUrl">
        </section>
    </section>
</template>
<script>
import upload from '../../../../components/common/upload'
import util from '../../../../assets/common/util'
import { mapGetters } from 'vuex'
export default {
    props: ['baseData', 'fileType'],
    data () {
        return {
            isOperate: true,
            base: {
                gameBigBg: '/static/images/game-bg.jpg',
                gameAnimateGif: '',
                gamePkAnimateGif: ''
            },
            imgShowUrl: '',
            pcImgStyle: {
                position: 'relative',
                height: '100%',
                width: 'auto',
                left: '50%',
                transform: 'translateX(-50%)'
            }
        }
    },
    mounted () {
        console.log(this.baseData)
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
            if (!this.base.gameBigBg) {
                this.$message({
                    message: '请添加游戏壁纸！',
                    type: 'warning'
                })
                return false
            }

            if (!this.base.gameAnimateGif) {
                this.$message({
                    message: '请添加过场动画！',
                    type: 'warning'
                })
                return false
            }

            if (!this.base.gamePkAnimateGif) {
                this.$message({
                    message: '请添加PK结果动画！',
                    type: 'warning'
                })
                return false
            }

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
        showImg (key) {
            var imgUrl = this.base[key]

            if (imgUrl && this.imgShowUrl != imgUrl) {
                this.imgShowUrl = imgUrl
            }
        },
        changeImgBg (data) {
            this.base.gameBigBg = data.url
            this.imgShowUrl = data.url
        },
        changeImgTr (data) {
            this.base.gameAnimateGif = data.url
            this.imgShowUrl = data.url
        },
        changeImgEn (data) {
            this.base.gamePkAnimateGif = data.url
            this.imgShowUrl = data.url
        }
    },
    components: {
        upload
    }
}
</script>