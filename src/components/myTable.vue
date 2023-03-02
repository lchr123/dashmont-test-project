<script>

var thisColumns = [
  {
    title: 'Category',
    key: 'category',
  },
  {
    title: 'Name',
    key: 'name',
  },
  {
    title: 'Users',
    key: 'users',
  },
  {
    title: 'Dashboards',
    key: 'dashboards',
  },
  {
    title: 'DeleteButton',
    key: 'deleteButton'
  }
]
var thisData = [
  {name: "Project A", users: 4, dashboards: 3, category: "D"},
  {name: "Project B", users: 2, dashboards: 4, category: "C"},
  {name: "Project C", users: 1, dashboards: 2, category: "F"},
  {name: "Project D", users: 3, dashboards: 2, category: "D"}
]
var categoryToColor = [
  {category: "C", color: "#f50"},
  {category: "D", color: "#2db7f5"},
  {category: "F", color: "#87d068"},
]
var searchValue=''
export default {
    name: 'myTable',
    data () {
      return {
        searchValue,
        thisData,
        thisColumns
      }
    },
    methods: {
      deleteItem(name) {
        // filter through all items
        this.thisData = this.thisData.filter((item) => {
          return item.name !== name;
        })
        console.log(this.thisData)
      },
      onSearch(searchContent) {
        // match by regx with ambiguity, if name contains searchContent, return true
        var pattern = new RegExp('.*' + searchContent + '.*','i');
        var matchedItems = [];
        this.thisData.forEach((item) => {
          if(pattern.test(item.name) === true) {
            matchedItems.push(item);
          }
          else {
            console.log(searchContent)
          }
        })
        this.thisData = matchedItems
      }
    }
}
</script>

<template>
  <a-input-search
    v-model:value="searchValue"
    placeholder="input search text"
    style="width: 200px; position: relative; left: 55%; margin-top: 10px; margin-bottom: 10px;"
    @search="onSearch"
  />
  <a-table :dataSource="thisData" :columns="thisColumns">
    <template #bodyCell="{ record, column }">
      <template v-if="column.key === 'category'">
        <template v-if="record.category === 'C'">
          <a-tag type="primary" color="#f50">{{ record.category }}</a-tag>
        </template>
        <template v-if="record.category === 'D'">
          <a-tag type="primary" color="#2db7f5">{{ record.category }}</a-tag>
        </template>
        <template v-if="record.category === 'F'">
          <a-tag type="primary" color="#87d068">{{ record.category }}</a-tag>
        </template>
      </template>
      <template v-else-if="column.key === 'name'">
        {{ record.name }}
      </template>
      <template v-else-if="column.key === 'users'">
        {{ record.users }} users
      </template>
      <template v-else-if="column.key === 'dashboards'">
        <a-tag color="green">{{ record.dashboards }} dashboards</a-tag>
      </template>
      <template v-else-if="column.key === 'deleteButton'">
        <a-popconfirm
          title="Are you sure delete this task?"
          ok-text="Yes"
          cancel-text="No"
          @confirm="deleteItem(record.name)"
          @cancel="cancel"
        >
          <a href="#" style="color: crimson;">Delete</a>
        </a-popconfirm>
      </template>
    </template>
  </a-table>
</template>

<style scoped>

</style>