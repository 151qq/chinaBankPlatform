<template>
    <div class="survey-list-box duan-font-box">
        <div class="input-box">
            <input
                placeholder="请输入需查询条件"
                v-model="keyValue"
                @keyup.13="searchItem"
                class="input-search">
            <el-button class="search-btn" type="primary" icon="search" @click="searchItem">
              搜索
            </el-button>

            <el-button class="add-new-btn" type="primary" icon="plus" @click="addItem">增加</el-button>
        </div>
        <section class="big-cards-box">
            <router-link class="card-box"
                         target="_blank"
                         v-for="(item, index) in marketList"
                         :to="{
                            name: 'survey-detail',
                            query: {
                              enterpriseCode: item.enterpriseCode,
                              subjectCode: item.subjectCode
                          }}">
                
                <div class="card-content">
                    {{item.subjectContent}}
                </div>
                <section class="card-btns">
                        <el-button v-if="item.subjectStatus != '1'"
                            class="list-btn" type="info" :plain="true" size="small"
                            @click.prevent="submitItem(item)">发布</el-button>

                        <el-button class="list-btn" type="danger" :plain="true" size="small"
                            @click.prevent="deleteItem(item)">删除</el-button>

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
  </div>
</template>
<script>
import util from '../../../assets/common/util'
import { mapGetters } from 'vuex'

export default {
    data () {
        return {
            isPage: false,
            keyValue: '',
            marketList: [],
            pageSize: 20,
            pageNumber: 1,
            total: 0
        }
    },
    mounted () {
         this.getList()
    },
    computed: {
        ...mapGetters({
            userInfo: 'getUserInfo'
        }),
        isEditor () {
          // return this.roleCodes.indexOf('product_admin') > -1
          return true
        }
    },
    methods: {
        searchItem () {
          this.getList()
        },
        getList () {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.subjectContent) {
                formData.subjectContent = this.keyValue
            }

            util.request({
                method: 'post',
                interface: 'subjectList',
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
        addItem () {
            var pathUrl = {
              name: 'survey-detail',
              query: {
                enterpriseCode: this.$route.query.enterpriseCode
              }
            }

            this.$router.push(pathUrl)
        },
        submitItem (item) {
            util.request({
              method: 'post',
              interface: 'submitSubject',
              data: {
                enterpriseCode: this.$route.query.enterpriseCode,
                subjectCode: item.subjectCode,
                subjectStatus: '1'
              }
            }).then(res => {
              if (res.result.success == '1') {
                this.getList()

                this.$message({
                  type: 'success',
                  message: '发布成功!'
                })
              } else {
                this.$message.error(res.result.message)
              }
            })
        },
        deleteItem (item) {
            util.request({
              method: 'get',
              interface: 'deleteSubject',
              data: {
                enterpriseCode: this.$route.query.enterpriseCode,
                subjectCode: item.subjectCode
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
        pageChange (size) {
            this.pageNumber = size
            this.getList()
        }
    }
}
</script>
<style lang="scss">
.survey-list-box {
    width: 1000px;
    margin: 80px auto 30px;
}
</style>