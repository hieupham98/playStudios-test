<script setup>
import GameItem from './components/Game/Item.vue'
import Filters from './components/Filters.vue'
import { ref, watch, onBeforeMount } from 'vue'

const title = ref('Playstudios Game Idea Hub')
const description = ref('Playstudios Game Idea Hub')

useHead({
  title,
  meta: [{
    name: 'description',
    content: description
  }]
})
const ideas = ref([])
const sortByVote = ref('')
const searchKey = ref('')
const loading = ref(true)

onBeforeMount(() => {
    loading.value = true;

})

onMounted(() => {
    const savedIdeas = localStorage.getItem('game-ideas');

    if (savedIdeas) {
        ideas.value = JSON.parse(savedIdeas);
        loading.value = false;
    }
})

const handleSortByVote = (value) => {
    sortByVote.value = value
}
const handleSearch = (value) => {
    searchKey.value = value
}

watch(sortByVote, async (newSort, oldSort) => {

    
    loading.value = true;

    let  savedIdeas = localStorage.getItem('game-ideas') ;

    if(savedIdeas) {
        savedIdeas = JSON.parse(savedIdeas);
        if(newSort === 'desc') {
            savedIdeas = savedIdeas.sort((a, b) => b.vote - a.vote);
        } 
        if(newSort === 'asc') 
        {
            savedIdeas = savedIdeas.sort((a, b) => a.vote - b.vote);
        }

        if(searchKey.value) {
            savedIdeas = savedIdeas.filter(item => item.desc.includes(searchKey.value));

        }

        ideas.value = savedIdeas;
    }

    loading.value = false;
})

watch(searchKey, async (newKw, oldKw) => {
    let  savedIdeas = localStorage.getItem('game-ideas') ;

    if(savedIdeas) {
        savedIdeas = JSON.parse(savedIdeas);

        savedIdeas = savedIdeas.filter(item => item.desc.includes(newKw));


        if(sortByVote.value === 'desc') {

            savedIdeas = savedIdeas.sort((a, b) => b.vote - a.vote);



        } 
        if(sortByVote.value === 'asc') 
        {
            savedIdeas = savedIdeas.sort((a, b) => a.vote - b.vote);
        }

        ideas.value = savedIdeas;
    }
})

</script>

<template>
    <el-row class="mb-5">
        <Filters @sortByVote="handleSortByVote" @search="handleSearch" />
    </el-row>
    <div class="mb-5 grid grid-cols-1 mx-3 md:mx-0 gap-1 md:grid-cols-4 md:gap-4">
        <div v-for="idea in ideas"> 
            <GameItem :loading="loading" :key="idea.id" :ideas="ideas" :idea="idea" />
        </div>
    </div>
</template>
