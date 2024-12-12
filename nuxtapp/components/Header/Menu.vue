<template>
    <el-menu
      :default-active="activeIndex"
      class="w-[100%] menu-main fixed top-0 left-0"
      mode="horizontal"
      :ellipsis="false"
    >

        <el-menu-item index="home">
            <NuxtLink to="/">
                <img alt="Logo" class="logo h-[50px]" src="@/assets/logo.png"   />
            </NuxtLink>
        </el-menu-item>
        <NuxtLink to="/" class="mr-3">
            <el-menu-item index="home">
                <Icon name="material-symbols:videogame-asset-outline-rounded" class="mr-2"/>
                Games</el-menu-item>
        </NuxtLink>
        <div class="justify-center mr-4 flex flex-col">
            <NuxtLink to="/create">
                <el-button> 
                    <Icon name="ep:plus" class="mr-1" />
                    <span class="hidden md:block">
                        Submit idea 
                    </span>
                </el-button> 
            </NuxtLink>
        </div>

        <!-- <div v-if="currentUser" class="hidden md:block justify-center mr-4 flex flex-col">
            {{ currentUser}}
        </div> -->

        <div v-if="currentUser" class="justify-center mr-4 flex flex-col">
            <el-tooltip
                :content="`Hello, ${currentUser}`"
            >
                <el-avatar
                    class="cursor-pointer"
                    src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
                />
            </el-tooltip>
        </div>
        <div v-if="currentUser" class="cursor-pointer justify-center mr-4 flex flex-col">
            <Icon @click="logout" name="material-symbols:logout" />
        </div>

        <div v-if="!currentUser" class="justify-center mr-4 flex flex-col">
            <el-popover
                :visible="visible"
                placement="bottom"
                title="Enter your name to login"
                :width="300"
            >
                <template #reference>
                    <el-button class="m-2" @click="visible = !visible">
                        <Icon name="lucide:log-in" class="mr-2"/>
                        Login
                    </el-button>
                </template>
                <slot>
                    <el-form-item  prop="name">
                        <el-input v-model="userName" />
                    </el-form-item>
                    <el-button @click="login">
                        <Icon name="lucide:check" class="mr-2"/>
                        Login
                    </el-button>
                    
                    <el-button @click="visible = false">
                        <Icon name="lucide:x" class="mr-2" />
                        Cancel
                    </el-button>
                </slot>
            </el-popover>
        </div>
    </el-menu>
</template>

<script lang="ts" setup>
    import { reactive, ref } from 'vue'
    const visible = ref(false)

    const authedvisible = ref(false)
    const userName = ref('')
    const activeIndex = ref('1')

    
    const currentUser = ref(null)

    const login = () => {
        localStorage.setItem('current-user', userName.value);

        currentUser.value = userName
        window.location.reload()

        ElMessage({
            message: 'Login succesfully.',
            grouping: true,
            type: 'success',
        })
    }

    const logout = () => {
        localStorage.removeItem('current-user');
        currentUser.value = null

        window.location.reload()
        ElMessage({
            message: 'Logout succesfully.',
            grouping: true,
            type: 'success',
        })
    }

    onMounted(() => {
        const user = localStorage.getItem('current-user');

        if (user) {
            currentUser.value = user;
        }
    })
</script>

<style>
    .el-menu--horizontal > .el-menu-item:nth-child(1) {
    margin-right: auto;
    }   
    .menu-main {
        z-index: 10000 !important;
    }
</style>
