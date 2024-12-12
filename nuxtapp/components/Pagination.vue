<script setup>
    const props = defineProps(['ideas'])


</script>

<template>
    <div class="example-pagination-block">
      <el-pagination  
        :page-size="5" 
       layout="prev, pager, next" :total="ideas.length"  />
    </div>
</template>

<style scoped>
.example-pagination-block + .example-pagination-block {
margin-top: 10px;
}
.example-pagination-block .example-demonstration {
margin-bottom: 16px;
}
</style>