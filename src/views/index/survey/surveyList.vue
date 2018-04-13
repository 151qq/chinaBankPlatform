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
                    <i class="el-icon el-icon-delete2"
                        v-if="isEditor"
                        @click.prevent="deleteItem(item)"></i>
                </section>
            </router-link>
        </section>

        <section class="null-box" v-if="!marketList.length && isPage">
          暂无内容！！！
        </section>
        <div class="more-load"
                v-if="total && marketList.length < total"
                @click="loadMore">加载更多...</div>
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
            marketList: [
              { 
                enterpriseCode: 20180401133451260,
                subjectCode: '22298838373773',
                subjectContent: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. Proin gravida dolor sit amet lacus accumsan et viverra justo commodo. Proin sodales pulvinar tempor. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Nam fermentum, nulla luctus pharetra vulputate, felis tellus mollis orci, sed rhoncus sapien nunc eget.'
              },
              { 
                enterpriseCode: 20180401133451260,
                subjectCode: '22298838373773',
                subjectContent: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean euismod bibendum laoreet. Proin gravida dolor sit amet lacus accumsan et viverra justo commodo. Proin sodales pulvinar tempor. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Nam fermentum, nulla luctus pharetra vulputate, felis tellus mollis orci, sed rhoncus sapien nunc eget.'
              }
            ],
            pageSize: 20,
            pageNumber: 1,
            total: 0
        }
    },
    mounted () {
         // this.getList()
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
        getList (type) {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                surveyType: this.surveyType,
                surveyScenario: this.userInfo.userCode,
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
            }

            if (this.keyValue) {
                formData.keyValue = this.keyValue
            }

            util.request({
                method: 'get',
                interface: 'selectByEcAndTy',
                data: formData
            }).then(res => {
                if (res.result.success == '0') {
                    this.$message.error(res.result.message)
                    return
                }

                this.total = Number(res.result.total)
                this.isPage = true
                if (!type) {
                    this.marketList = res.result.result
                } else {
                    this.marketList = this.marketList.concat(res.result.result)
                }
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
        deleteItem (item) {
            util.request({
              method: 'get',
              interface: 'deleteSurvey',
              data: {
                surveyCode: item.surveyCode
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
        loadMore () {
            this.pageNumber++
            this.getList('more')
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