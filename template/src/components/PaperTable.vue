<template>
  <div>
    <div class="header">
      <slot name="header">
        <h4 class="title">{{title}}</h4>
        <p class="category">{{subTitle}}</p>
      </slot>
    </div>
    <div class="table-responsive">
      <table class="table" :class="tableClass">
        <thead>
          <th v-for="column in columns" align='center'>{{columnify(column)}}</th>
        </thead>
        <tbody>
          <tr v-for="item in data">
            <td v-for="column in columns" v-if="hasValue(item, column)">{{column==='amount'? moneify(itemValue(item, column)):itemValue(item, column)}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class='paginator' v-if="pagination">
      <span v-if="page>2" @click="pageChange(1)">{{ $t('message.first') }}</span>
      <span v-if="page>1" @click="pageChange(page-1)">{{ page-1 }}</span>
      <span class="active">{{ page }}</span>
      <span v-if="page!=totalPages" @click="pageChange(page+1)">{{ page+1 }}</span>
      <span v-if="page!=totalPages" @click="pageChange(totalPages)">{{ $t('message.last') }}</span>
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      columns: Array,
      data: Array,
      type: {
        type: String, // striped | hover
        default: 'striped'
      },
      title: {
        type: String,
        default: ''
      },
      subTitle: {
        type: String,
        default: ''
      },
      pagination: {
        default: false
      },
      max_results: 5,
      total: 0,
      page: 1,
      totalPages: null
    },
    computed: {
      tableClass () {
        return `table-${this.type}`
      }
    },
    methods: {
      hasValue (item, column) {
        return item[column.toLowerCase()] !== 'undefined'
      },
      itemValue (item, column) {
        return item[column.toLowerCase()]
      },
      moneify (value) {
        return this.$n(value, 'currency')
      },
      columnify (value) {
        value = value.replace('_', ' ')
        value = value.replace('id', '')
        value = value.charAt(0).toUpperCase() + value.slice(1)
        // value[0] = value[0].toUpperCase()
        return value
      },
      percentify (value) {
        return this.$n(value, 'percentage')
      },
      pageChange (page) {
        this.$emit('pageChange', page)
      }
    }
  }
</script>
<style scoped>
  th{
    text-align: center;
  }
</style>