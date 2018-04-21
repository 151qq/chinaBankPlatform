<template>
  <div class="small-imgs-box">
    <section class="img-box" v-for="(item, index) in attachmentData" :key="index">
      <img class="img-big" :src="item.attachmentSourceCode">
      <div class="bg-black" @click="showImg(index)"></div>
      <div class="delete-box" v-if="isOperate">
        <i class="el-icon-close" @click.self="deleteImg(index)"></i>
      </div>
    </section>
    <label v-if="isOperate && imgNum && attachmentData.length < imgNum" class="img-box" @click="showMedia">
      <img class="img-big" src="../../assets/images/add-img.jpg">
    </label>

    <el-dialog
      title="上传"
      class="upload-dia"
      :visible.sync="dialogVisible"
      size="tiny">
      <section>
        <label class="input-label" :for="idFor">本地上传</label>
        <input type="file" accept="image/*" class="ben-input" :id="idFor" @change="postImg($event)">

        <el-button @click="getMediaList('2')" class="upload-btn" type="primary">图片素材</el-button>
      </section>
    </el-dialog>

    <file-lists :doc-type="docType"
                :select-data="selectData"
                @suSelect="suSelect"></file-lists>

    <swiper-attachment :is-show="isShow" :index="nowIndex" :big-imgs="attachmentData"></swiper-attachment>
  </div>
</template>
<script>
import util from '../../assets/common/util'
import fileLists from './fileLists'
import swiperAttachment from './swiper-attachment.vue'

export default {
    props: ['attachmentData', 'attachmentType', 'isOperate', 'subjectCode', 'isPopup', 'imgNum'],
    data() {
      return {
        idFor: 'upload-file-single',
        dialogVisible: false,
        mediaList: [],
        enterprise: '',
        docType: '',
        selectData: {
          isShow: false
        },
        nowIndex: 0,
        isShow: {
          value: false
        }
      }
    },
    methods: {
      showMedia () {
        if (this.isPopup) {
          this.$emit('imgClick')
          return
        }

        console.log(1)

        this.dialogVisible = true
      },
      getMediaList (type) {
        this.docType = type
        this.selectData.isShow = true
      },
      suSelect (imgData) {
        this.selectData.isShow = false
        this.dialogVisible = false
        var data = {
          attachmentTargetType: this.attachmentType,
          attachmentSourceType: "attachmen_type_8",
          attachmentSourceCode: imgData.url
        }
        this.attachmentData.push(data)
      },
      postImg (e) {
        var opotion = {
          url: 'uploadFile',
          event: e,
          data: {
            enterpriseCode: this.$route.query.enterpriseCode,
            fileType: '2'
          }
        }

        util.uploadFile(opotion).then(res => {
          if (res.result.success == '1') {
            let result = res.result.result
            var data = {
              attachmentTargetType: this.attachmentType,
              attachmentSourceType: "attachmen_type_1",
              attachmentSourceCode: result.filePath
            }
            this.dialogVisible = false
            this.attachmentData.push(data)
          } else {
            this.$message.error(res.result.message)
          }
        })
      },
      deleteImg (index) {
        this.attachmentData.splice(index, 1)
      },
      showImg (index) {
        this.nowIndex = index
        this.isShow.value = true
      }
    },
    components: {
      fileLists,
      swiperAttachment
    }
}
</script>

<style lang="scss">
.small-imgs-box {
  width: 100%;
  overflow: hidden;

  .img-box {
    float: left;
    position: relative;
    margin-right: 10px;
    margin-bottom: 10px;
    position: relative;
    cursor: pointer;
    width: 80px;
    height: 60px;
    overflow: hidden;

    .img-big {
      position: relative;
      left: 50%;
      top: 0;
      width: auto;
      min-width: 100%;
      height: 100%;
      transform: translateX(-50%);
    }

    .bg-black {
      display: none;
      position: absolute;
      top: 0px;
      left: 0px;
      width: 100%;
      height: 100%;
      background: #000000;
      opacity: 0.3;
      z-index: 1;
    }

    .delete-box {
      display: none;
      position: absolute;
      top: 0px;
      right: 3px;
      text-align: right;
      font-size: 12px;
      line-height: 20px;
      cursor: pointer;
      color: #ffffff;
      z-index: 2;

      i {
        margin-left: 3px;

        &:hover {
          color: #20a0ff;
        }
      }
    }

    &:hover {
      .delete-box {
        display: block;
      }

      .bg-black {
        display: block;
      }
    }
  }
}

.upload-dia {
  .input-label {
    display: block;
    width: 100%;
    color: #fff;
    background-color: #20a0ff;
    border: 1px solid #20a0ff;
    padding: 10px 15px;
    border-radius: 4px;
    text-align: center;
    line-height: 1;
    white-space: nowrap;
    cursor: pointer;
    margin: 0 0 15px 0;

    &:hover {
      opacity: 0.8;
    }
  }

  .ben-input {
    display: none;
  }

  .upload-btn {
    width: 100%;
    margin: 0 0 15px 0;
  }
}
</style>