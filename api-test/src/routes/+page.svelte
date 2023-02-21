<script>
    import {onMount} from 'svelte';
    import hobbyStore from "./hobby-store";

    let hobbies = [];
    let hobbyInput;
    let isLoading = false;


    onMount(() => {
        fetch('https://svelte-course-214ac-default-rtdb.firebaseio.com/hobbies.json')
        .then(res => {
            isLoading = false;
            if(!res.ok){
                throw new Error("에러임");
            }
            return res.json();    
        }).then(data => {
            console.log(data, "data");
            hobbyStore.setHobbies(Object.values(data));
        })
        .catch(err => {
            isLoading = false;
            console.log(err);
        })
    })
    

    function addHobby(){
        hobbyStore.addHobby(hobbyInput.value);

        isLoading = true;
        fetch('https://svelte-course-214ac-default-rtdb.firebaseio.com/hobbies.json', {
            method: 'POST',
            body: JSON.stringify(hobbyInput.value),
            headers: {
                'Content-Type': 'application/json'
            }
        }).then(res => {
            isLoading = false;
            if(!res.ok){
                throw new Error("에러임");
            }
            alert("저장완료");
        }).catch(err => {
            isLoading = false;
            console.log(err);
        })
    }
</script>


<label for="hobby">hobby</label>
<input type="text" id="hobby" bind:this={hobbyInput}>
<button on:click={addHobby}>Add hobby</button>

{#if isLoading}
    <p>로딩중...</p>
{:else}
    <ul>
        {#each $hobbyStore as hobby }
            <li>{hobby}</li>
        {/each}
    </ul>
{/if}
