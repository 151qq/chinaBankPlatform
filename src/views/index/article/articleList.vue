<template>
    <div class="article-list-box duan-list-box">
        <div class="btn-input-box">
            <el-button class="add-new-btn"
                       type="primary"
                       icon="plus"
                       v-if="isEdit"
                       @click="addItem">增加</el-button>
        </div>
        <section class="big-cards-box">
            <router-link class="card-box"
                         v-for="(item, index) in marketList"
                         target="_blank"
                         :to="{
                                name: item.pageStatus == '2' ? 'article-detail' : 'article-info',
                                query: {
                                  enterpriseCode: item.enterpriseCode,
                                  pageCode: item.pageCode,
                                  templateCode: item.templateCode
                                }
                              }">
                <div class="card-img">
                    <img v-if="item.pageCover" :src="item.pageCover">
                </div>
                
                <div class="card-content">
                    <div class="card-title">{{item.pageTitle}}</div>
                    <div class="card-desc">{{item.pageAbstract}}</div>
                    <div class="card-tag">
                        <el-tag v-if="item.pageStatus == '2'" type="gray">草稿</el-tag>
                        <el-tag v-if="item.pageStatus == '1'" type="success">已发布</el-tag>
                        <el-tag v-if="item.pageStatus == '3'">已下架</el-tag>
                    </div>
                </div>
                <section class="card-btns" v-if="isEdit">
                    <el-button v-if="item.pageStatus == '2'" type="danger" :plain="true" size="small"
                        @click.prevent="deleteItem(item)">删除</el-button>

                    <el-button v-if="item.pageStatus == '2'" type="info" :plain="true" size="small"
                        @click.prevent="changeStatus(item, '1')">发布</el-button>

                    <el-button v-if="item.pageStatus == '1'" type="info" :plain="true" size="small"
                        @click.prevent="changeStatus(item, '3')">终止</el-button>
                </section>
            </router-link>
        </section>

        <section class="null-box" v-if="!marketList.length && isPage">
          暂无内容！！！
        </section>

        <el-pagination
            v-if="total"
            class="page-box"
            @current-change="pageChange"
            layout="prev, pager, next"
            :page-size="pageSize"
            :total="total">
        </el-pagination>

        <el-dialog title="添加文章" :visible.sync="isAddItem">
          <el-form :label-position="'left'" :model="addItemForm" label-width="80px">
            <el-form-item label="文章标题">
                <el-input v-model="addItemForm.pageTitle"
                          auto-complete="off"
                          placeholder="请输入内容,最多40个字"
                          :maxlength="40"></el-input>
            </el-form-item>
            <el-form-item label="文章摘要">
                <el-input
                    type="textarea"
                    :rows="3"
                    placeholder="请输入内容"
                    :maxlength="70"
                    v-model="addItemForm.pageAbstract">
                </el-input>
                <div class="limit-box">剩余<a>{{pageAbstractNum}}</a>字</div>
            </el-form-item>
            <el-form-item label="文章封面">
                <popup-img :path="addItemForm.pageCover"
                            :is-operate="isEdit"
                            :bg-path="false"
                            @imgClick="imgClick"></popup-img>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
                <el-button @click="isAddItem = false">取 消</el-button>
                <el-button type="primary" @click="confirmItem">确 定</el-button>
          </div>
        </el-dialog>

        <popup-load :path="addItemForm.pageCover"
                     :is-operate="true"
                     :bg-path="true"
                     :id-name="'pageCover'"
                     :is-upload="isUpload"
                     @changeImg="changeItemImg"></popup-load>
  </div>
</template>
<script>
import util from '../../../assets/common/util'
import popupImg from '../../../components/common/popupImg.vue'
import popupLoad from '../../../components/common/popupLoad.vue'
import { mapGetters } from 'vuex'

export default {
    props: ['articleType'],
    data () {
        return {
            isPage: false,
            keyValue: '',
            marketList: [],
            pageSize: 20,
            pageNumber: 1,
            total: 0,
            isAddItem: false,
            addItemForm: {
                pageTitle: '',
                pageCover: '',
                pageAbstract: ''
            },
            isUpload: {
                value: false
            }
        }
    },
    mounted () {
      this.getList()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEdit () {
          if (this.$route.query.enterpriseCode == this.userInfo.enterpriseCode && this.articleType == 'template_type_1') {
            return true
          } else {
            return false
          }
        },
        pageAbstractNum () {
          return 70 - this.addItemForm.pageAbstract.length
        }
    },
    watch: {
      $route () {
        this.keyValue = ''
        this.pageNumber = 1
        this.getList()
      }
    },
    methods: {
        searchItem () {
          this.getList()
        },
        getList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                pageType: this.articleType,
                pageEditor: this.userInfo.userCode,
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.keyValue) {
                formData.keyValue = this.keyValue
            }

            util.request({
                method: 'post',
                interface: 'html5PageList',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.total = Number(res.result.total)
                this.isPage = true
                this.marketList = res.result.result
            })
        },
        changeStatus (item, type) {
            util.request({
              method: 'post',
              interface: 'html5PageSubmit',
              data: {
                pageCode: item.pageCode,
                pageStatus: type
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()
                this.$message({
                  type: 'success',
                  message: '更新成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
            })
        },
        deleteItem (item) {
            util.request({
              method: 'post',
              interface: 'html5PageDelete',
              data: {
                pageCode: item.pageCode
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()

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
            this.addItemForm = {
                pageTitle: '',
                pageCover: '',
                pageAbstract: '',
                pageEditor: this.userInfo.userCode
            }
            this.isAddItem = true
        },
        imgClick () {
            this.isUpload.value = true
        },
        changeItemImg (data) {
            this.addItemForm.pageCover = data.url
        },
        confirmItem () {
            if (!this.addItemForm.pageTitle) {
              this.$message.error('文章标题不能为空！')
              return
            }

            if (this.addItemForm.pageAbstract.length < 40) {
              this.$message.error('文章摘要最少40个字！')
              return
            }

            if (!this.addItemForm.pageCover) {
              this.$message.error('文章封面不能为空！')
              return
            }

            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                pageType: this.articleType,
                pageTitle: this.addItemForm.pageTitle,
                pageCover: this.addItemForm.pageCover,
                pageAbstract: this.addItemForm.pageAbstract,
                pageEditor: this.userInfo.userCode
            }

            util.request({
                method: 'post',
                interface: 'html5PageSave',
                data: formData
            }).then(res => {
                if (res.result.success == '1') {
                  this.pageNumber = 1
                  this.getList()
                  this.isAddItem = false
                } else {
                  this.$message.error(res.result.message)
                }
            })
        },
        pageChange (size) {
            this.pageNumber = size
            this.getList('more')
        }
    },
    components: {
        popupImg,
        popupLoad
    }
}
</script>
<style lang="scss">
.article-list-box {
    margin: auto;
}
</style>