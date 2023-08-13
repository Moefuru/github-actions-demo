<template>
  <!--  筛选框  -->
  <div class="mt-12">
    <el-input
        v-model="keyword"
        placeholder="请输入关键词搜索"
        class="input-with-select"
        @input="filterTableData"
    >
      <template #prepend>
        <el-select
            v-model="select"
            clearable="true"
            placeholder="分类"
            style="width: 115px"
            @change="filterTableData"
        >
          <el-option
              v-for="item in categoryData"
              :key="item.id"
              :label="item.name"
              :value="item.id"
          />
        </el-select>
      </template>
      <template #append>
        <el-button :icon="Search"/>
      </template>
    </el-input>
  </div>
  <!--  筛选列表  -->
  <div class="mt-12">
    <el-table :data="tableData" height="250" style="width: 100%">
      <el-table-column label="分类" width="100">
        <template #default="{row}">
          <a :href="row.category_id ?? 0" target="_blank">
            {{ categoryMap[row.category_id] ?? '' }}
          </a>
        </template>
      </el-table-column>
      <el-table-column prop="name" label="名称" width="260"/>
      <el-table-column label="访问链接">
        <template #default="{row}">
          <a :href="row.url ?? '#'" target="_blank">
            {{ row.url ?? '' }}
          </a>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>


<script setup lang="ts">
import {ref, onMounted} from 'vue'
import {Search} from '@element-plus/icons-vue'
import axios from "axios";

const keyword = ref('')
const select = ref('')

const categoryData = ref([])
const tableData = ref([])
const categoryMap = ref([])

onMounted(async () => {
  await getCategoryList()
  await filterTableData()
});

// 获取分类列表
async function getCategoryList() {
  try {
    const response = await axios.get('/api/category-list')
    console.log('getCategoryList', response)
    categoryData.value = response.data.data
    categoryMap.value = response.data.data.reduce((acc, cur) => {
      acc[cur.id] = cur.name
      return acc
    }, {})
    console.log('categoryMap', categoryMap.value)
  } catch (error) {
    console.error('Error fetching data from API:', error)
  }
}

// 筛选列表
async function filterTableData() {
  try {
    const response = await axios.get('/api/tool-list', {
      params: {
        keyword: keyword.value,
        category_id: select.value,
      },
    });

    tableData.value = response.data.data;
    console.log('filterTableData', tableData.value)
  } catch (error) {
    console.error('Error filtering data from API:', error);
  }
}


</script>

<style>
.input-with-select .el-input {
  height: 40px; /* Adjust the height value as needed */
}

.input-with-select .el-input-group__prepend {
  background-color: var(--el-fill-color-blank);
  height: 40px; /* Adjust the height value to match the input height */
  display: flex;
  align-items: center;
}
</style>
