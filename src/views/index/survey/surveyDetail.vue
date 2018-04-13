<template>
    <div class="form-discount">
        <el-collapse v-model="activeNames" @change="collChange">
          <el-collapse-item class="float-form-box" title="题目信息" name="0">
            <survey-base></survey-base>
          </el-collapse-item>

          <template v-if="$route.query.subjectCode">
            <div class="line-bold"></div>
            <el-collapse-item class="float-form-box" title="题目标签" name="1">
              <subject-tag></subject-tag>
            </el-collapse-item>
            <div class="line-bold"></div>
            <el-collapse-item class="float-form-box" title="题目选项" name="2">
              <anwsers></anwsers>
            </el-collapse-item>
          </template>
        </el-collapse>          
    </div>
</template>
<script>
import util from '../../../assets/common/util'
import surveyBase from './formAlist/surveyBase'
import subjectTag from './formAlist/subjectTag'
import anwsers from './formAlist/anwsers'

export default {
    data () {
        return {
            activeNames: ['0'],
            baseData: {}
        }
    },
    mounted () {
        var surveyColl = localStorage.getItem("surveyColl")
        if (surveyColl) {
            this.activeNames = surveyColl.split(',')
        }
    },
    methods: {
        collChange () {
            localStorage.setItem("surveyColl", this.activeNames)
        }
    },
    components: {
        surveyBase,
        subjectTag,
        anwsers
    }
}
</script>