<script>
    import Header from "./UI/Header.svelte";
    import MeetupItem from "./Meetups/MeetupItem.svelte";
    import MeetupGrid from "./Meetups/MeetupGrid.svelte";

    let title = '';
    let subTitle = '';
    let address = '';
    let email = '';
    let description = '';
    let imageUrl = '';
    let meetups = [
        {
            id: 'm1',
            title: 'Coding Bootcamp',
            subTitle: 'learn to code',
            description: 'in this meetup',
            imageUrl: '',
            address:'27th',
            contactEmail:'korea4127@gmail.com'
        },
        {
            id: 'm2',
            title: 'Swim 투게더',
            subTitle: 'learn to code',
            description: 'in this meetup',
            imageUrl: '',
            address:'27th',
            contactEmail:'korea4127@gmail.com'
        },
        {
            id: 'm3',
            title: '후후후후p',
            subTitle: 'learn to code',
            description: 'in this meetup',
            imageUrl: '',
            address:'27th',
            contactEmail:'korea4127@gmail.com'
        },
        {
            id: 'm4',
            title: '이직을 해보자',
            subTitle: 'learn to code',
            description: 'in this meetup',
            imageUrl: '',
            address:'27th',
            contactEmail:'korea4127@gmail.com'
        }
    ]

    function addMeetup(){
        // meetups.push({
        //     id: Math.random(),
        //     title,
        //     subTitle,
        //     address,
        //     email,
        //     description,
        //     imageUrl
        // }); // 작동 안됨
        const newMeetup =  {
            id: Math.random(),
            title,
            subTitle,
            address,
            email,
            description,
            imageUrl
        };
        meetups = [newMeetup, ...meetups]
    }
</script>
<style>
    main {
        margin-top: 5rem;
    }
</style>
<Header/>
<main>
    <form on:submit|preventDefault={addMeetup}>
        <div class="form-control">
            <label for="title">Title</label>
            <input type="text" id="title" bind:value={title}>
        </div>
        <div class="form-control">
            <label for="title">subTitle</label>
            <input type="text" id="subTitle" bind:value={subTitle}>
        </div>
        <div class="form-control">
            <label for="address">address</label>
            <input type="text" id="address" bind:value={address}>
        </div>
        <div class="form-control">
            <label for="imageUrl">imageUrl</label>
            <input type="text" id="imageUrl" bind:value={imageUrl}>
        </div>
        <div class="form-control">
            <label for="email">email</label>
            <input type="email" id="email" bind:value={email}>
        </div>
        <div class="form-control">
            <label for="description">description</label>
            <textarea rows="3" id="description" bind:value={description}/>
        </div>
        <button type="submit">Save</button>
    </form>
    <MeetupGrid {meetups}/>
</main>