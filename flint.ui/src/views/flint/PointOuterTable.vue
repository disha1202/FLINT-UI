<template>
  <div class="outer-table">
    <div class="top-row">
      <div :class="{ active: isTable }" @click="isTable = true">Table</div>
      <div :class="{ active: !isTable }" @click="isTable = false">Graph</div>
    </div>
    <div class="full-w">
      <PointOutput v-if="!isTable" />
      <Table v-else :columns="columns" :rows="rows" />
    </div>
  </div>
</template>

<script>
/* eslint-disable no-alert, no-console */

import PointOutput from './PointOutput.vue'
import Table from './Table.vue'
export default {
  name: 'PointOuterTable',
  components: {
    PointOutput,
    Table
  },
  data() {
    return {
      isTable: true,
      columns: [
        {
          title: 'Step',
          dataIndex: 'point_step',
          key: 'point_step'
        },
        {
          title: 'Step Date',
          dataIndex: 'point_stepDate',
          key: 'point_stepDate'
        },
        {
          title: 'Step Length(years)',
          dataIndex: 'point_stepLenInYears',
          key: 'point_stepLenInYears'
        },
        {
          title: 'Pool 1',
          dataIndex: 'pool_1',
          key: 'pool_1'
        },
        {
          title: 'Pool 2',
          dataIndex: 'pool_2',
          key: 'pool_2'
        },
        {
          title: 'Pool 3',
          dataIndex: 'pool_3',
          key: 'pool_3'
        }
      ],
      rows: this.generateDataRows()
    }
  },
  beforeCreate() {
    this.$store.dispatch('parse_point_results')
    this.isTable = true
  },
  methods: {
    pool_1() {
      return this.$store.state.point.pool_1
    },
    pool_2() {
      return this.$store.state.point.pool_2
    },
    pool_3() {
      return this.$store.state.point.pool_3
    },
    point_step() {
      return this.$store.state.point.point_step
    },
    point_stepDate() {
      return this.$store.state.point.point_stepDate
    },
    point_stepLenInYears() {
      return this.$store.state.point.point_stepLenInYears
    },
    generateDataRows: function () {
      var result = []
      var pool_1 = this.pool_1(),
        pool_2 = this.pool_2(),
        pool_3 = this.pool_3(),
        point_step = this.point_step(),
        point_stepDate = this.point_stepDate(),
        point_stepLenInYears = this.point_stepLenInYears()
      for (let j = 0; j < point_step.length; j++) {
        let original_date = point_stepDate[j]
        let date = original_date.substring(0, 10) + ' ' + original_date.substring(11)
        var defaultDateFormat = new Date(date)
        let row = {
          key: j + 1,
          point_step: point_step[j] + 1,
          point_stepDate: defaultDateFormat.toISOString().substring(0, 10),
          point_stepLenInYears: point_stepLenInYears[j].toPrecision(5),
          pool_1: pool_1[j].toPrecision(5),
          pool_2: pool_2[j].toPrecision(5),
          pool_3: pool_3[j].toPrecision(5)
        }

        result.push(row)
      }
      return result
    }
  }
}
</script>

<style>
.outer-table {
  border: 1px solid #475447;
  border-radius: 5px;
}

.top-row {
  display: flex;
  flex-direction: row;
  border-bottom: 2px solid #eeeef0;
}

.top-row div {
  width: 50%;
  text-align: center;
  padding: 16px;
  cursor: pointer;
}

.top-row .active {
  color: white;
  background-color: #475447;
}
</style>
