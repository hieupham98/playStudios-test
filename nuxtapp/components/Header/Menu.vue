<template>
    <el-menu
      :default-active="activeIndex"
      class="w-[100%] fixed  top-0 left-0"
      mode="horizontal"
      :ellipsis="false"
      @select="handleSelect"
    >

    <el-menu-item index="home">
        <NuxtLink to="/">
            <img alt="Logo" class="logo h-[50px]" src="@/assets/logo.png"   />
        </NuxtLink>
    </el-menu-item>
    <NuxtLink to="/" class="mr-3">
        <el-menu-item index="home">Games</el-menu-item>
    </NuxtLink>
    <div class="justify-center mr-4 flex flex-col">
        <NuxtLink to="/create">
            <el-button> 
                <Icon name="ep:plus" class="mr-1" />
                 Submit idea 
            </el-button> 
        </NuxtLink>
    </div>

    <div v-if="currentUser" class="justify-center mr-4 flex flex-col">
       Action with {{ currentUser}}
    </div>

    <div v-if="currentUser" class="justify-center mr-4 flex flex-col">

                <el-avatar
                    src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
                />


    </div>
    <div v-if="currentUser" class="cursor-pointer justify-center mr-4 flex flex-col">
        <Icon @click="logout" name="material-symbols:logout" />
    </div>

    <div v-if="!currentUser" class="justify-center mr-4 flex flex-col">
        <el-popover
            :visible="visible"
            placement="bottom"
            title="Input your name to login"
            :width="300"
        >
            <template #reference>
                <el-button class="m-2" @click="visible = !visible">
                    Login
                </el-button>
            </template>
            <slot>
                <el-form-item  prop="name">
                    <el-input v-model="userName" />
                </el-form-item>
                <el-button @click="login">
                    Login
                </el-button>
                
                <el-button @click="visible = false">
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
        console.log(userName)
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
</style>

<style>
.el-menu-demo {
    /* background: #282828; */
}
</style>
