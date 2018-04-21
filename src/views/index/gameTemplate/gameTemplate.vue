<template>
    <section class="game-template-box">
        <el-tabs v-model="activeName" v-if="isLoad">
            <el-tab-pane :label="'基本信息'" :name="'base'">
                <game-base :file-type="'base'" :base-data="base" @getBase="getBase"></game-base>
            </el-tab-pane>
            <el-tab-pane :label="'开始页'" :name="'start'">
                <game-start :file-type="'start'" :base-data="base" @getBase="getBase"></game-start>
            </el-tab-pane>
            <el-tab-pane :label="'游戏页'" :name="'play'">
                <game-play :file-type="'play'" :base-data="base" @getBase="getBase"></game-play>
            </el-tab-pane>
            <el-tab-pane :label="'结束页'" :name="'stop'">
                <game-stop :file-type="'stop'" :base-data="base" @getBase="getBase"></game-stop>
            </el-tab-pane>
            <el-tab-pane :label="'分享页'" :name="'share'">
                <game-share :file-type="'share'" :base-data="base" @getBase="getBase"></game-share>
            </el-tab-pane>
            <el-tab-pane :label="'助威'" :name="'help'">
                <game-help :file-type="'help'" :base-data="base" @getBase="getBase"></game-help>
            </el-tab-pane>
            <el-tab-pane :label="'PK'" :name="'pk'">
                <game-pk :file-type="'pk'" :base-data="base" @getBase="getBase"></game-pk>
            </el-tab-pane>
        </el-tabs>
    </section>
</template>
<script>
import util from '../../../assets/common/util'
import gameBase from './formAlist/gameBase'
import gameStart from './formAlist/gameStart'
import gamePlay from './formAlist/gamePlay'
import gameStop from './formAlist/gameStop'
import gameShare from './formAlist/gameShare'
import gameHelp from './formAlist/gameHelp'
import gamePk from './formAlist/gamePk'
import { mapGetters } from 'vuex'

export default {
    data () {
        return {
            isLoad: false,
            isOperate: true,
            activeName: 'base',
            base: {}
        }
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          return this.$route.query.enterpriseCode == this.userInfo.enterpriseCode
        }
    },
    mounted () {
        this.getBase()
    },
    methods: {
        getBase () {
            util.request({
                method: 'get',
                interface: 'gameTemplateDetail',
                data: {
                    enterpriseCode: this.$route.query.enterpriseCode,
                    templateCode: this.$route.query.templateCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    var obj = res.result.result

                    if (obj) {
                        for (var key in obj) {
                            if (!obj[key]) {
                                delete obj[key]
                            }
                        }
                    }

                    this.base = obj
                    this.isLoad = true
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
    },
    components: {
        gameBase,
        gameStart,
        gamePlay,
        gameStop,
        gameShare,
        gameHelp,
        gamePk
    }
}
</script>
<style lang="scss">
.game-template-box {
    width: 1060px;
    margin: 80px auto 30px;
    overflow: hidden;

    .el-color-picker {
        width: 200px;

        .el-color-picker__trigger {
            height: 30px;
            padding: 3px;
        }
    }

    .block-title {
        clear: both;
        font-size: 16px;
        color: #000000;
        line-height: 30px;
        font-weight: bold;
        margin-bottom: 20px;
    }

    .left-content {
        float: left;
        width: 640px;
    }

    .right-content {
        position: relative;
        display: flex;
        flex-direction: column;
        overflow: hidden;
        box-sizing: border-box;
        border: 1px solid #bfcbd9;
        border-radius: 3px;
        height: 600px;
        float: right;
        width: 350px;
        margin-top: 30px;
    }
}
</style>