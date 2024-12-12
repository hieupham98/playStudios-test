<script setup>
import VoteButton from './VoteButton.vue'
const props = defineProps(['idea', 'ideas'])

</script>

<template>
    <el-card  style="max-width: 480px">
        <template #header>
            <div class="flex justify-between"> 
                <div class="flex flex-col justify-center game-title-idea">
                    <b>{{ idea.name }} {{ idea.id }}</b> 
                </div>
                <VoteButton :ideas="ideas" :idea="idea" />

            </div>
            <div class="flex"> 
                <div class="game-title-idea">
                    {{ idea.desc }}
                </div>
            </div>
        </template>
        <img
        src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png"
        style="width: 100%"
        />
    </el-card>
</template>