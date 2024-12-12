<script setup>
import VoteButton from './VoteButton.vue'
const props = defineProps(['idea', 'ideas', 'loading'])


</script>

<template>
    <el-skeleton style="width: 240px" :loading="loading" animated>
      <template #template>
        <el-skeleton-item variant="image" style="width: 240px; height: 240px" />
        <div style="padding: 14px">
          <el-skeleton-item variant="h3" style="width: 50%" />
          <div
            style="
              display: flex;
              align-items: center;
              justify-items: space-between;
              margin-top: 16px;
              height: 16px;
            "
          >
            <el-skeleton-item variant="text" style="margin-right: 16px" />
            <el-skeleton-item variant="text" style="width: 30%" />
          </div>
        </div>
      </template>
      <template #default>
        <el-card  class="mt-3 md:mt-0" style="max-width: 480px">
            <template #header>
                <div class="flex justify-between"> 
                    <div class="flex flex-col justify-center game-title-idea">
                        <div class="flex flex-row">
                            <div class="flex mr-2 flex-col justify-center">
                                <Icon name="lucide:lightbulb" />
                            </div>
                            <b>{{ idea.name }}</b> 
                        </div>
                    </div>
                    <VoteButton :ideas="ideas" :idea="idea" />

                </div>
                <div class="flex idea-text-desc"> 
                    <div class="text-wrap cursor-pointer">
                        <el-popover
                            placement="top-start"
                            title="Description"
                            :width="300"
                            trigger="hover"
                            :content="idea.desc"
                        >
                            <template #reference>
                                {{ idea.desc }}
                            </template>
                        </el-popover>

                    </div>
                </div>
            </template>
            <el-row>
                <img
                    src="/assets/image.png"
                    style="width: 100%"
                />
            </el-row>
            <el-row class="justify-center mt-3">idea from  {{ idea.user }}</el-row> 
        </el-card>
      </template>
    </el-skeleton>
</template>
<style lang="css">

.text-wrap {
  width: 300px; 
  height: 100px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box; 
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
}
</style>