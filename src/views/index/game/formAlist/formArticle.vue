<template>
    <section class="article-box-outer">
      <el-button class="add-btn"
                type="primary"
                size="small"
                icon="plus"
                v-if="base.eventStatus == '1' || base.eventStatus == '2'"
                @click="addItem">增加</el-button>
      <router-link class="card-box"
                    target="_blank"
                   v-for="(item, index) in selectList"
                   :to="{name: 'article-detail', query: {enterpriseCode: item.enterpriseCode, pageCode: item.pageCode, templateCode: item.templateCode}}">
          <div class="card-img">
              <img v-if="item.pageCover" :src="item.pageCover">
          </div>

          <div class="card-content">
              <div class="card-title">{{item.pageTitle}}</div>
              <div class="card-desc">{{item.pageAbstract}}</div>
          </div>
          
          <section class="card-btns"
                    v-if="base.eventStatus == '1' || base.eventStatus == '2'">
              <i class="el-icon-delete2"
                  @click.prevent="deleteItem(item)"></i>
          </section>
      </router-link>
      <div v-if="!selectList.length"
            class="null-box">
        请点击增加按钮添加！
      </div>

      <el-dialog class="select-big-box" title="选择文章" :visible.sync="dialogVisible">
          <div class="articles-box" v-for="(item, index) in articleList"
              @click="changeSelect(item)"
              :class="selectData.indexOf(item.pageCode) > -1 ? 'active' : ''">
            <div class="card-img">
                <img v-if="item.pageCover" :src="item.pageCover">
            </div>

            <div class="card-content">
                <div class="card-title">{{item.pageTitle}}</div>
                <div class="card-desc">{{item.pageAbstract}}</div>
            </div>
          </div>
          <div v-if="!articleList.length"
                class="null-box">
            暂无可推广文章，请增加文章！
          </div>
          <el-pagination
            layout="prev, pager, next"
            v-if="total > articleList.length"
            :page-size="pageSize"
            @current-change="changePage"
            :total="total">
          </el-pagination>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="confirmSelect">确 定</el-button>
          </div>
        </el-dialog>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
export default {
    props: ['base'],
    data () {
        return {
          articleList: [],
          pageSize: 2,
          pageNumber: 1,
          total: 0,
          defaultSelect: [],
          selectData: [],
          selectList: [],
          dialogVisible: false
        }
    },
    mounted () {
      this.getArticles()
      this.getSelectList()
    },
    methods: {
        getSelectList () {
          var formData = {
              enterpriseCode: this.$route.query.enterpriseCode,
              eventCode: this.$route.query.eventCode,
              pageSize: 100,
              pageNumber: 1
          }

          util.request({
              method: 'get',
              interface: 'eventpagemapList',
              data: formData
          }).then(res => {
              if (res.result.success == '1') {
                var arrs = []

                if (!res.result.result) {
                  return
                }

                if (res.result.result.length) {
                  res.result.result.forEach((item) => {
                    arrs.push(item.pageCode)
                  })
                }
              
                this.defaultSelect = [].concat(arrs)
                this.selectData = arrs
                this.selectList = res.result.result
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        getArticles () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                pageType: 'template_type_1',
                pageStatus: '1',
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            util.request({
                method: 'post',
                interface: 'html5PageList',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                  this.total = Number(res.result.total)
                  this.articleList = res.result.result
                } else {
                  this.$message.error(res.result.message)
                }
            })
        },
        deleteItem (item) {
          var formData = {
              enterpriseCode: this.$route.query.enterpriseCode,
              eventCode: this.$route.query.eventCode,
              pageCode: item.pageCode
          }

          util.request({
              method: 'get',
              interface: 'eventpagemapDelete',
              data: formData
          }).then(res => {
              if (res.result.success == '1') {
                this.getSelectList()
                this.$message({
                  type: 'success',
                  message: '删除成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        addItem () {
          this.pageNumber = 1
          this.selectData = [].concat(this.defaultSelect)
          this.dialogVisible = true
        },
        changeSelect (item) {
          var index = this.selectData.indexOf(item.pageCode)
          if (index > -1) {
              this.selectData.splice(index, 1)
          } else {
              this.selectData.push(item.pageCode)
          }
        },
        confirmSelect () {
          var formData = {
              enterpriseCode: this.$route.query.enterpriseCode,
              eventCode: this.$route.query.eventCode,
              pageCodes: this.selectData.join(',')
          }

          util.request({
              method: 'post',
              interface: 'eventpagemapCreate',
              data: formData
          }).then(res => {
              if (res.result.success == '1') {
                this.getSelectList()
                this.dialogVisible = false
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        changePage (size) {
          this.pageNumber = size
          this.getArticles()
        }
    }
}
</script>
<style lang="scss">
.article-box-outer {

  .el-dialog--small {
    width: 478px;
  }

  .card-btns {
    text-align: right;
    position: absolute;
    font-size: 16px;
    color: #000000;
    right: 10px;
    bottom: 23px;
  }

  .card-box {
    position: relative;
    display: block;
    overflow: hidden;
    padding-bottom: 20px;
    border-bottom: 1px solid #C0CCDA;
    margin-bottom: 20px;

    .card-img {
      float: left;
      width: 200px;
      height: 120px;
      background: #cfcfd0;

      img {
        display: block;
        width: 200px;
        height: 120px;
      }
    }

    .card-content {
      float: right;
      width: 780px;

      .card-title {
        font-size: 18px;
        line-height: 30px;
        color: #000000;
      }

      .card-desc {
        font-size: 14px;
        line-height: 24px;
        color: #475669;
        height: 48px;
        margin-top: 10px;
        overflow: hidden;
      }
    }
  }

  .null-box {
    line-height: 80px;
    padding-bottom: 20px;
  }

  .articles-box {
    width: 438px;
    heigth: 96px;
    padding: 8px 10px;
    box-sizing: border-box;
    overflow: hidden;
    margin-bottom: 14px;
    background: url(../../../../assets/images/check-icon.png) 398px center no-repeat;
    background-color: #EFF2F7;
    cursor: pointer;

    &.active {
      background: url(../../../../assets/images/check-now.png) 398px center no-repeat;
      background-color: #EFF2F7;
    }

    .card-img {
      float: left;
      width: 120px;
      height: 70px;
      background: #cfcfd0;

      img {
        display: block;
        width: 120px;
        height: 70px;
      }
    }

    .card-content {
      float: left;
      width: 246px;
      margin-left: 15px;

      .card-title {
        font-size: 16px;
        line-height: 26px;
        color: #000000;
      }

      .card-desc {
        font-size: 14px;
        line-height: 22px;
        color: #475669;
        height: 44px;
        overflow: hidden;
      }
    }
  }
}
</style>