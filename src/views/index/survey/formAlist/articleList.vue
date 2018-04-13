<template>
    <section>
        <router-link class="small-card-box"
                             v-for="(item, index) in articleList"
                             target="_blank"
                             :to="{
                                name: 'article-info',
                                query: {
                                  enterpriseCode: $route.query.enterpriseCode,
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
            </div>

            <section class="card-btns" v-if="isEdit">
                <i class="el-icon el-icon-delete2"
                    @click.prevent="deleteItem(index)"></i>
            </section>
        </router-link>
        <div class="null-box-small" v-if="!articleList.length">
            暂无附件
        </div>
    </section>
</template>
<script>
export default {
    props: ['articleList', 'isEdit'],
    data () {
        return {}
    },
    methods: {
        deleteItem (index) {
            this.$emit('deleteArticle', index)
        }
    }
}
</script>