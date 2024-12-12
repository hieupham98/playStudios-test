<script setup>
    import { ref, computed  } from 'vue'
    const props = defineProps(['idea', 'ideas'])

    const idea = props.idea
    const ideas = props.ideas

    const currentVotes = ref([])
    const userVoted = ref(0)
    const totalVote = ref(0)



    onMounted(() => {
        const votes = localStorage.getItem('votes');
        const user = localStorage.getItem('current-user');

        if (votes) {
            currentVotes.value = JSON.parse(votes);
            userVoted.value = findVote(currentVotes.value, user, idea.id)

            totalVote.value = getTotalVotesByIdea(currentVotes.value, idea.id)
        }

    })
    // const totalVote = computed(() => {
    //     console.log(currentVotes.value)
    //     const filteredVotes = currentVotes.value.filter(record => record.idea === idea);
        
    //     const totalVotes = filteredVotes.reduce((total, record) => total + record.vote, 0);
        
    //     return totalVotes;
    // })

    function updateVoteForItem(items, updatedItem) {
        return items.map(item => {
            if (item.id === updatedItem.id) {
                item.vote = updatedItem.vote;  
            }
            return item;
        });
    }

    function getTotalVotesByIdea(votes, idea) {
    // Lọc các bản ghi theo idea
        const filteredVotes = votes.filter(record => record.idea === idea);
        
        // Tính tổng vote cho idea
        const totalVotes = filteredVotes.reduce((total, record) => total + record.vote, 0);
        
        return totalVotes;
    }

    const  findVote = (votes, user, idea) => {
        const voteRecord = votes.find(record => record.user === user && record.idea === idea);
        return voteRecord ? voteRecord.vote : null
    }
    function handleVote(user, idea, action) {
        let votes = localStorage.getItem('votes');

        if(votes) {
            votes = JSON.parse(votes);
        }

        const index = votes.findIndex(vote => vote.user === user && vote.idea === idea);

        if (index !== -1) {
            if (votes[index].vote === action) {
                // Nếu đã thực hiện cùng hành động, xóa vote
                votes.splice(index, 1);
            } else {
                // Nếu hành động khác (upvote -> downvote hoặc ngược lại)
                votes[index].vote = action;
            }
        } else {
            // Nếu chưa vote, thêm mới
            votes.push({ user, idea, vote: action });
        }


        return votes;
    }
    const onVote = (action) => {
        const user = localStorage.getItem('current-user');


        if(user)  {
            const newVotes = handleVote(user, idea.id, action) 


            localStorage.setItem('votes', JSON.stringify(newVotes))
            userVoted.value = findVote(newVotes, user, idea.id)
            totalVote.value = getTotalVotesByIdea(newVotes, idea.id)

            let newIdeas = updateVoteForItem(ideas, { id: idea.id, vote: totalVote.value });

            // console.log(newIdeas)

            localStorage.setItem('game-ideas', JSON.stringify(newIdeas));

        } else {
            ElMessage({
            message: 'Please login before take the action',
                grouping: true,
                type: 'error',
            })
        }
    } 
</script>


<template>
    <div class="flex flex-col justify-center text-center leading-[0.5px]">
        <div @click="onVote(1)" :class="`cursor-pointer ${userVoted === 1 ? 'text-blue-700': 'text-gray-400'} text-lg`">
            <Icon name="ep:caret-top" />
        </div>
        <div>
            <b class="text-gray-400">
                {{ totalVote }}
            </b>
        </div>
        <div @click="onVote(-1)" :class="`cursor-pointer ${userVoted === -1 ? 'text-blue-700': 'text-gray-400'} text-lg`">
            <Icon name="ep:caret-bottom" />
        </div>
    </div>
</template>