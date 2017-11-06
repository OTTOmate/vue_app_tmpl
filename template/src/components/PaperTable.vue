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

      }
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
      }
    }
  }
</script>
<style>
  th{
    text-align: center;
  }
</style>