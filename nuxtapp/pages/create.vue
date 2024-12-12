<script setup>
    import CreateForm from './components/Game/CreateForm.vue'
    import { ref } from 'vue'

    const title = ref('Create idea')
    const description = ref('Playstudios Game Idea Hub')

    useHead({
        title,
        meta: [{
            name: 'description',
            content: description
        }]
    })
</script>


<template>
    <div class="container">
        <div class="mx-auto">
            <el-row class="justify-center mb-5">
                <h2 class="text-md">
                    <Icon name="lucide:lightbulb" />
                    Submit a new idea
                </h2>
            </el-row>
            <div class="max-w-[700px]  mx-3 md:mx-auto justify-center">
                <CreateForm />
            </div>
        </div>
    </div>
</template>

