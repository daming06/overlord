<template>
  <div>
    <p class="job-page__title">Job 列表</p>
    <el-table :data="jobList" ref="dataTable" v-loading="loading" border @current-change="onSelectionChanged">
      <el-table-column type="expand">
        <template slot-scope="{ row }">
          <vue-json-pretty v-if="row.param" :data="JSON.parse(row.param)"></vue-json-pretty>
        </template>
      </el-table-column>
      <el-table-column type="index" width="80">
      </el-table-column>
      <el-table-column prop="id" label="Id">
      </el-table-column>
      <el-table-column prop="state"
        label="State"
        :filters="stateFilters"
        :filter-method="filterTag"
        filter-placement="bottom-end">
        <template slot-scope="{ row }">
          <el-tag>{{ row.state }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import VueJsonPretty from 'vue-json-pretty'
import { mapState, mapGetters } from 'vuex'

export default {
  components: {
    VueJsonPretty
  },
  computed: {
    ...mapState({
      jobList: state => state.jobs.all,
      loading: state => state.jobs.loading
    }),
    ...mapGetters('jobs', {
      stateFilters: 'jobStateList'
    })
  },
  created () {
    this.$store.dispatch('jobs/getAllJobs')
  },
  methods: {
    filterTag (value, row) {
      return row.state === value
    },
    onSelectionChanged (newRow) {
      const table = this.$refs.dataTable
      table.toggleRowExpansion(newRow)
      table.setCurrentRow()
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/style/mixin.scss';

.job-page__title {
  @include page-title-font;
  margin: 10px 0;
}
</style>

<style lang="scss">
.vjs__tree {
  font-size: 12px;
}
</style>
