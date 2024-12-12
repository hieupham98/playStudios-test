<template>
    <div class="ml-3">
        <el-select
        v-model="value"
        clearable
        @change="handleSortByVote"
        placeholder="Sort by vote"
        style="width: 240px"
        >
        <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
        />
        </el-select>
        <el-input
        v-model="searchKey"
        style="width: 240px"
        class="mt-2 md:mt-0 md:ml-5"
        @change="handleSearch"
        placeholder="Please Input"
        :suffix-icon="Search"
        />
    </div>
</template>

  <script lang="ts" setup>
  import { ref } from 'vue'
  import { Search } from '@element-plus/icons-vue'
  const emit = defineEmits(['sortByVote', 'search']);

  const value = ref('')
  const searchKey = ref('')
  const options = [
    {
      value: 'asc',
      label: 'Asc',
    },
    {
      value: 'desc',
      label: 'Desc',
    },
  ]

  const handleSortByVote  = () => {
    emit('sortByVote', value.value)
  }

  const handleSearch  = () => {
    emit('search', searchKey.value)
  }
  </script>
  