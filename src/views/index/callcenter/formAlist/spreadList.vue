<template>
    <section class="product-base-box">
      <el-button class="add-btn" type="primary"
                icon="plus" size="small"
                @click="addBarrier">
        增加
      </el-button>
      
      <section class="inForm" v-for="(item, index) in barrieList" :key="index">
        <div class="formDiscount">
          <section class="formBox">
              <span>推广渠道类型</span>
              <el-select
                class="input-box"
                v-model="item.adChannelType"
                filterable
                placeholder="请选择">
                <el-option
                  v-for="(item, index) in spreadTypes"
                  :key="index"
                  :label="item.typeName"
                  :value="item.id">
                </el-option>
              </el-select>
          </section>
          <section class="formBox">
            <span>推广渠道名称</span>
            <el-input
              class="input-box"
              placeholder="请输入内容,最16个字"
              :maxlength="16"
              v-model="item.adChannelName">
            </el-input>
          </section>
          <section class="formBox bigF">
            <span>推广渠道描述</span>
            <el-input
              type="textarea"
              :rows="4"
              placeholder="请输入内容,最140个字"
              :maxlength="140"
              v-model="item.adChannelDesc">
            </el-input>
            <div class="limit-box">剩余<a>{{140 - item.adChannelDesc.length}}</a>字</div>
          </section>
          <section class="formBox" v-if="item.adQrcode">
            <span>渠道二维码</span>
            <div class="input-box">
              <upload :path="item.adQrcode"
                      :bg-path="false"
                      :is-operate="false"
                      :item-index="index"></upload>
            </div>
          </section>       
        </div>
        <el-button class="save-btn" type="danger" :plain="true" size="small" icon="delete2"
            @click="deleteBase(item, index)">删除</el-button>

        <el-button class="save-btn" type="info" :plain="true" size="small" icon="document"
            @click="saveBase(item)">保存</el-button>
        <div class="clear"></div>
      </section>

      <div v-if="!barrieList.length" class="null-page">
            暂无渠道
      </div>
    </section>
</template>
<script>
import util from '../../../../assets/common/util'
import upload from '../../../../components/common/upload'
export default {
    data () {
        return {
            barrieList: [],
            spreadTypes: []
        }
    },
    mounted () {
      this.getList()
      this.getTypes()
    },
    methods: {
        getTypes () {
            util.request({
                method: 'get',
                interface: 'disOfEnterpriseInfo',
                data: {
                    types: 'marketing_ad_type'
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.spreadTypes = res.result.result.marketing_ad_type
                } else {
                    this.$message.error(res.result.message)
                }
            })
        },
        getList () {
          util.request({
              method: 'get',
              interface: 'getOrganizeAdChannelListByCode',
              data: {
                enterpriseCode: this.$route.query.enterpriseCode
              }
          }).then(res => {
              if (res.result.success == '1') {
                this.barrieList = res.result.result
              } else {
                this.$message.error(res.result.message)
              }
          })
        },
        addBarrier () {
          this.barrieList.unshift({
            enterpriseCode: this.$route.query.enterpriseCode,
            adChannelType: '',
            adChannelCode: '',
            adChannelName: '',
            adChannelDesc: ''
          })
        },
        deleteBase (barrieData, index) {
          if (!barrieData.adChannelCode) {
            this.barrieList.splice(index, 1)
          } else {
            util.request({
                method: 'get',
                interface: 'deleteOrganizeAdChannel',
                data: {
                  adChannelCode: barrieData.adChannelCode
                }
            }).then(res => {
                if (res.result.success == '1') {
                    this.getList()
                } else {
                    this.$message.error(res.result.message)
                }
            })
          }
        },
        saveBase (barrieData) {
            if (!barrieData.adChannelType) {
                this.$message({
                    message: '请选择推广渠道类型！',
                    type: 'warning'
                })
                return false
            }

            if (barrieData.adChannelName.trim() == '') {
                this.$message({
                    message: '请填写推广渠道名称！',
                    type: 'warning'
                })
                return false
            }

            // if (barrieData.adChannelDesc.trim() == '') {
            //     this.$message({
            //         message: '请填写推广渠道描述！',
            //         type: 'warning'
            //     })
            //     return false
            // }
            
            util.request({
                method: 'post',
                interface: 'organizeAdChannelOperate',
                data: barrieData
            }).then(res => {
                if (res.result.success == '1') {
                    this.$message({
                      type: 'success',
                      message: '保存成功!'
                    })

                    this.getList()
                } else {
                    this.$message.error(res.result.message)
                }
            })
        }
    },
    components: {
      upload
    }
}
</script>
<style lang="scss">
.product-base-box {
  overflow: hidden;

  .form-box {
    overflow: hidden;
  }

  .inForm {
    padding: 20px 0;
    border-bottom: 1px solid #99A9BF;

    &:last-child {
      border: none;
      padding-bottom: 0;
    }

    &:nth-child(2) {
      padding-top: 0;
    }
  }
}
</style>