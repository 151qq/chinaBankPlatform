<template>
    <section class="game-template-box">
        <el-tabs v-model="activeName" v-if="isLoad">
            <!-- <el-tab-pane :label="'基本信息'" :name="'base'">
                <game-base :file-type="'base'" :base-data="base" @getBase="getBase"></game-base>
            </el-tab-pane> -->
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
            <!-- <el-tab-pane :label="'助威'" :name="'help'">
                <game-help :file-type="'help'" :base-data="base" @getBase="getBase"></game-help>
            </el-tab-pane> -->
            <!-- <el-tab-pane :label="'PK'" :name="'pk'">
                <game-pk :file-type="'pk'" :base-data="base" @getBase="getBase"></game-pk>
            </el-tab-pane> -->
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
            activeName: 'start',
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

    .game-comment-box {
        margin-top: 20px;

        .comment-b {
            margin-bottom: 20px;
            display: flex;

            .avatar-box {
                width: 36px;
                height: 36px;
                background: url(/static/images/head-icon.png) center no-repeat;
                background-size: 100%;
                margin-right: 10px;
                overflow: hidden;
                border-radius: 50%;

                img {
                    display: block;
                    width: 100%;
                    height: 100%;
                }
            }

            .content-box {
                flex: 1;
                padding: 10px;
                background: #ffffff;
                border-radius: 3px;
                overflow: hidden;
                box-shadow: 2px 2px 10px -4px #999999;


                .title-box {
                    display: flex;
                    align-items: center;
                    justify-content: space-between;

                    .title {
                        flex: 1;
                        font-weight: 400;
                        font-size: 16px;
                        line-height: 20px;
                        color: #2B2B41;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        white-space: nowrap;
                        word-wrap: normal;
                        word-wrap: break-word;
                        word-break: break-all;
                    }

                    .btn-box {
                        width: 30px;
                        text-align: right;
                        font-size: 14px;
                        color: #FA746E ;
                    }
                }

                .des-box {
                    font-size: 16px;
                    line-height: 20px;
                    color: #7E7E90;
                    margin-top: 7px;
                }

                .imgs-box {
                    margin-top: 7px;
                }
            }
        }
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
        background-position: center top;
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-color: #F9F9F9;

        .game-big-body {
            display: flex;
            flex-direction: column;
            width: 100%;
            flex: 1;
            background-position: center top;
            background-repeat: no-repeat;
            background-size: 100% 100%;
        }

        .clock-box {
            font-size: 40px;
            line-height: 53px;
            margin-top: 17px;
            font-weight: bold;
            text-align: center;
        }

        .game-desc-box {
            padding: 0 30px;
            font-size: 16px;
            margin-top: 30px;
            line-height: 22px;
            color: #2B2B41;
        }

        .start-btn-box {
            width: 100%;
            flex-direction: column;
            display: flex;
            align-items: center;
            box-sizing: border-box;
            overflow: hidden;
            margin-bottom: 70px;
        }

        .play-btn-box {
            width: 100%;
            display: flex;
            align-items: center;
            height: 50px;

            span {
              color: #999999;
            }

            .play-btn {
              flex: 1;
              display: flex;
              align-items: center;
              justify-content: center;

              img {
                display: inline-block;
                height: 14px;
                margin-right: 10px;
              }
            }
        }

        .logo-box {
            position: absolute;
            right: 20px;
            bottom: 20px;
            width: 90px;
            height: 20px;
            background: url(/static/images/logo.png) no-repeat center;
            background-size: auto 100%;
          }
        }

        .gmBodyArea {
            flex: 1;
            display: block;
            padding: 0 20px;
            box-sizing: border-box;
            overflow: auto;

            .share-box {
              height: 100%;
              display: flex;
              flex-direction: column;
              justify-content: space-around;

              .share-title {
                margin-top: 20px;

                img {
                  display: block;
                  width: 80%;
                  margin: auto;
                }
              }

              .person-box {
                .attar-box {
                  width: 140px;
                  height: 140px;
                  overflow: hidden;
                  position: relative;
                  left: 50%;
                  transform: translateX(-50%);

                  .bg-attar {
                    display: block;
                    width: 100%;
                    height: 100%;
                  }

                  .attar {
                    position: absolute;
                    left: 50%;
                    top: 50%;
                    border-radius: 50%;
                    transform: translate(-50%, -50%);
                  }
                }

                .person {
                  text-align: center;

                  .name-box {
                    font-size: 14px;
                    color: #ffffff;
                    text-align: center;
                  }

                  .game-name {
                    font-size: 14px;
                    color: #ffffff;
                    background: url() no-repeat left top;
                    background-size: 100% 100%;
                    width: 180px;
                    height: 36px;
                    text-align: center;
                    margin: 10px auto;
                  }

                  .user-num {
                    height: 40px;
                    display: flex;
                    align-items: center;
                    font-size: 14px;
                    color: #ffffff ;
                    justify-content: center;

                    img {
                      display: block;
                      width: 18px;
                      height: 14px;
                      margin-right: 10px;
                    }

                    .num-box {
                      font-size: 16px;
                    }
                  }
                }
              }

              .ewm-box {
                img {
                  display: block;
                  width: 100px;
                  height: 100px;
                  margin: auto;
                }

                span {
                  display: block;
                  text-align: center;
                  line-height: 36px;
                  font-size: 12px;
                  color: #ffffff;
                }
              }
            } 

            .anwser-big-box {
                display: flex;
                justify-content: space-between;
                flex-wrap: wrap;
            }

        .anwser-box {
            display: flex;
            width: 100%;

            .select-box {
              width: 30px;

              img {
                display: block;
                width: 16px;
                height: 16px;
              }
            }
        }

        .anwser-box-half {
            width: 48%;
        }

        .anwser-option-box {
            width: 100%;
            height: 100px;
            overflow: hidden;
            position: relative;
            background: #000000;

            .title-bg {
              position: absolute;
              width: 100%;
              left: 0;
              bottom: 0;
              height: 30px;
              background: #000000;
              opacity: 0.6;
              z-index: 1;
            }    

            .question-title {
              width: 100%;
              position: absolute;
              left: 0;
              bottom: 0;
              font-size: 14px;
              text-align: center;
              line-height: 20px;
              color: #ffffff;
              z-index: 2;
            }

            .img-anwser {
              position: relative;
              height: 100%;
              width: auto;
              left: 50%;
              transform: translateX(-50%);
            }
        }

        .attachment-box {
            display: flex;
            align-items: center;

            .img-box {
              height: 100%;
              overflow: hidden;

              img {
                display: block;
                height: 100%;
                width: auto;
                margin-right: 10px;
              }
            }

            .attachment-content {
              flex: 1;
            }
        }

        .money-box {
            text-align: center;
            margin-top: 28px;
            line-height: 20px;
            color: #2B2B41;
            font-size: 14px;

            span {
              color: #7E7E90;
            }

            p {
              font-size: 12px;
              color: #7E7E90;
              margin-top: 10px;
              line-height: 17px;
            }
        }

        .honor-box {
            display: block;
            width: 320px;
            height: 89px;
            background: url(/static/images/ticket-bg.png) no-repeat left top;
            background-size: 100% 100%;
            margin-top: 20px;
            line-height: 89px;

            .money-num {
              float: left;
              width: 120px;
              text-align: center;
              font-size: 28px;
              color: #2B2B41;
            }

            .btn-font {
              float: right;
              width: 200px;
              text-align: center;
              font-size: 16px;
              color: #ffffff;

              img {
                display: inline-block;
                width: auto;
                height: 16px;
                margin-bottom: -2px;
                margin-right: 5px;
              }
            }
          }
        }

        .gmPlay {
          padding: 0 30px;
        }

        .person-message-box {
          position: relative;
          width: 100%;
          height: 240px;
          overflow: hidden;
          background-position: center;
          background-repeat: no-repeat;
          background-size: 100% auto;

          .person-box {
            padding-top: 30px;

            .attar-box {
              width: 60px;
              height: 60px;
              border-radius: 50%;
              overflow: hidden;
              margin: auto;
              margin-bottom: 10px;

              img {
                display: block;
                width: 100%;
                height: 100%;
              }
            }
        }

        .user-info-box {
            padding: 30px 20px;
            display: flex;

            .attar-box {
              width: 46px;
              height: 46px;
              border-radius: 50%;
              overflow: hidden;
              margin: auto;
              margin-right: 15px;
              border: 1px solid #ffffff;

              img {
                display: block;
                width: 100%;
                height: 100%;
              }
            }

            .user-content {
              flex: 1;

              .user-name {
                display: block;
                line-height: 25px;
                font-size: 18px;
                color: #ffffff;
              }

              .user-text {
                display: block;
                line-height: 21px;
                font-size: 12px;
                color: #ffffff;
              }
            }

            .user-info {
              height: 46px;
              display: flex;
              align-items: center;
              font-size: 14px;
              color: #ffffff ;

              img {
                display: block;
                width: 18px;
                height: 14px;
                margin-right: 10px;
              }

              .user-num {
                font-size: 18px;
              }
            }
        }

        .user-data-box {
            margin: 0 20px;
            padding-top: 60px;
            font-size: 40px;
            color: #ffffff;
            text-align: right;
          }
        }

        .person-pk-box {
          display: flex;
          position: relative;
          width: 100%;
          height: 240px;
          overflow: hidden;
          background-position: center;
          background-repeat: no-repeat;
          background-size: 100% 100%;

          .person-box {
            overflow: hidden;
            background-position: center;
            background-repeat: no-repeat;
            background-size: auto 100%;
            flex: 1;
            padding-top: 100px;

            .attar-box {
              width: 46px;
              height: 46px;
              border-radius: 50%;
              overflow: hidden;
              margin: auto;
              margin-bottom: 10px;
              border: 1px solid #ffffff;

              img {
                display: block;
                width: 100%;
                height: 100%;
              }
            }

            .title-box {
              text-align: center;
              font-size: 16px;
              color: #ffffff;
              line-height: 22px;
            }

            .message-box {
              text-align: center;
              font-size: 14px;
              color: #ffffff;
              line-height: 20px;
              margin-top: 7px;
            }
          }

          .money-box {
            font-size: 14px;
            padding-top: 172px;
            color: #ffffff;
          }
        }

        .bottom-btn-box {
          display: flex;
          height: 60px;
          margin: 0 5px;
          justify-content: space-between;
          align-items: center;

          .btn-box {
            flex: 1;
            margin: 0 5px;
            text-align: center;
          }
        }
}
</style>