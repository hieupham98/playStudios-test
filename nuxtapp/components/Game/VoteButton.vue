
<template>
    <div class="flex flex-col justify-center text-center leading-[0.5px]">
        <el-tooltip
            class="box-item"
            effect="dark"
            content="Upvote this idea"
            placement="top"
        >
            <div
                title="Upvote" 
                @click="onVote(1)" 
                :class="`cursor-pointer ${userVoted === 1 ? 'text-blue-700': 'text-gray-400'} text-lg`">
                <Icon name="ep:caret-top" />
            </div>
        </el-tooltip>
        <div>
            <b class="text-gray-400">
                {{ totalVote }}
            </b>
        </div>
        <el-tooltip
            class="box-item"
            effect="dark"
            content="Downvote this idea"
            placement="bottom"
        >
            <div
                title="Downvote" 
                @click="onVote(-1)" 
                :class="`cursor-pointer ${userVoted === -1 ? 'text-blue-700': 'text-gray-400'} text-lg`">
                <Icon name="ep:caret-bottom" />
            </div>
        </el-tooltip>
    </div>
</template>

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

    function updateVoteForItem(items, updatedItem) {
        return items.map(item => {
            if (item.id === updatedItem.id) {
                item.vote = updatedItem.vote;  
            }
            return item;
        });
    }

    function getTotalVotesByIdea(votes, idea) {
        const filteredVotes = votes.filter(record => record.idea === idea);
        
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
        } else {
            votes = []

        }

        const index = votes.findIndex(vote => vote.user === user && vote.idea === idea);

        if (index !== -1) {
            if (votes[index].vote === action) {
                votes.splice(index, 1);
            } else {
                votes[index].vote = action;
            }
        } else {
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

