<template>
    <div class="container">
        <div class="mx-auto">
            <el-row class="justify-center mb-5">
                <h2 class="text-md">
                    Submit your new idea
                </h2>
            </el-row>
            <el-row class="justify-center">
                <CreateForm />
            </el-row>
        </div>
        <!-- <p>This page will be displayed at the /about route.</p> -->
    </div>
</template>

<script setup>
import CreateForm from './components/Game/CreateForm.vue'

</script>
