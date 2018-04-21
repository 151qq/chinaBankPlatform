<template>
    <el-dialog
      title="选取文章"
      :visible="visibleData.show"
      size="small">
      <section>
        <div class="small-card-box"
            v-for="(item, index) in articleData"
            @click="checkSelect(item)">
            <div class="check-box">
                <img v-if="selectData.codes.indexOf(item.pageCode) < 0" src="../../assets/images/check-icon.png">
                <img v-else src="../../assets/images/check-now.png">
            </div>
            <div class="card-img">
                <img v-if="item.pageCover" :src="item.pageCover">
            </div>
            
            <div class="card-content">
                <div class="card-title">{{item.pageTitle}}</div>
                <div class="card-desc">{{item.pageAbstract}}</div>
            </div>
        </div>
        <div class="clear"></div>
        <el-pagination
            v-if="total"
            class="page-box"
            layout="prev, pager, next"
            @current-change="itemPageChange"
            :page-size="pageSize"
            :total="total">
        </el-pagination>
      </section>

        <div slot="footer" class="dialog-footer">
            <el-button @click="visibleData.show = false">取 消</el-button>
            <el-button type="primary" @click="confirmAttachment">确 定</el-button>
        </div>
    </el-dialog>
</template>
<script>
import util from '../../assets/common/util'

export default {
    props: ['visibleData', 'articleCode', 'articleList', 'maxLength'],
    data () {
        return {
            selectData: {
                codes: [],
                datas: []
            },
            articleData: [],
            pageSize: 5,
            pageNumber: 1,
            total: 0
        }
    },
    mounted () {
        this.getList()
    },
    methods: {
        initData () {
            var selectData = {
                codes: [].concat(this.articleCode),
                datas: [].concat(this.articleList)
            }

            this.selectData = selectData
        },
        getList (type) {
            var formData = {
                enterpriseCode: this.$route.query.enterpriseCode,
                pageType: 'template_type_1',
                pageSize: this.pageSize,
                pageNumber: this.pageNumber
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
                if (!type) {
                    this.articleData = res.result.result
                } else {
                    this.articleData = this.articleData.concat(res.result.result)
                }
            })
        },
        itemPageChange (size) {
            this.pageNumber = size
            this.getList()
        },
        confirmAttachment () {
            var selectData = Object.assign(this.selectData)
            this.visibleData.show = false
            this.$emit('getAttachments', selectData)
        },
        checkSelect (item) {
            var maxLength = this.maxLength || 1

            var index = this.selectData.codes.indexOf(item.pageCode)

            if (index > -1) {
                this.selectData.codes.splice(index, 1)
                this.selectData.datas.splice(index, 1)
            } else {
                if (this.selectData.codes.length >= maxLength) {
                    this.$message({
                        message: '最多选择' + maxLength + '个附件！',
                        type: 'warning'
                    })
                    return false
                }
                
                this.selectData.codes.push(item.pageCode)
                this.selectData.datas.push(item)
            }
        }
    }
}
</script>