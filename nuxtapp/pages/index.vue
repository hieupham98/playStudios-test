<script setup>
import GameItem from './components/Game/Item.vue'
import Menu from './components/Header/Menu.vue'
import Pagination from './components/Pagination.vue'
import { ref } from 'vue'

const ideas = ref([])

onMounted(() => {
    const savedIdeas = localStorage.getItem('game-ideas');

    if (savedIdeas) {
        ideas.value = JSON.parse(savedIdeas);
    }

})

</script>

<template>
    <div class="grid grid-cols-4 gap-4">

    <div v-for="idea in ideas"> 
        <GameItem :ideas="ideas" :idea="idea" />
    </div>
    </div>
    <el-row class="my-10">
        <Pagination  :ideas="ideas"/>
    </el-row>
</template>
