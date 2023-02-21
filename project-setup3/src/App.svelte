<script>
    import meetups from "./Meetups/meetups-sotre";
    import Header from "./UI/Header.svelte";
    import MeetupGrid from "./Meetups/MeetupGrid.svelte";
    import EditMeetup from "./Meetups/EditMeetup.svelte";
    import Button from "./UI/Button.svelte";
    import MeetupDetail from "./Meetups/MeetupDetail.svelte";
    import { onMount } from "svelte";
    import LoadingSpinner from "./UI/LoadingSpinner.svelte";
    import Error from "./UI/Error.svelte";
  

    let editMode;
    let editedId;
    let page = "overview";
    let pageData = {};
    let isLoading = true;
    let error;

    onMount(() => {
      fetch("https://svelte-course-214ac-default-rtdb.firebaseio.com/meetups.json")
      .then(res => {
        if(!res.ok){
          throw new Error("응답실패");
        }
        return res.json()
      })
      .then(data => {
        const loadedMeetups = [];
        for(const key in data){
          loadedMeetups.push({...data[key], id:key});
        }

        setTimeout(() => {
          isLoading = false;
          meetups.setMeetups(loadedMeetups.reverse());

        }, 1000);
      })
      .catch(err => {
        error = err;
        isLoading = false;
        console.error(err);
      })
    })

    function savedMeetup({detail}) {
      editMode = null;
      editedId = null;
    }

    function cancelEdit(){
      editMode = null;
      editedId = null;
    }

    function showDetails(event){
      page = "details";
      pageData.id = event.detail; 
      console.log(event, "event")
    }

    function closeDetails(){
      page = "overview";
      pageData = {};
    }

    function startEdit(event){
      editMode = 'edit';
      editedId = event.detail;
    }

    function clearError(){
      error = null;
    }
  </script>
  
  <style>
    main {
      margin-top: 5rem;
    }
  
    .meetup-controls{
      margin: 1rem;
    }
  </style>
  
  {#if error}
  <Error message={error?.message} on:cancel={clearError}/>
  {/if}

  <Header />
  
  <main>
    {#if page === 'overview'}

      {#if editMode === 'edit'}
      <EditMeetup id={editedId} on:save="{savedMeetup}" on:cancel={cancelEdit}/>
      {/if}
      {#if isLoading}
        <LoadingSpinner/>
      {:else}
        <MeetupGrid meetups={$meetups} on:showdetails={showDetails} on:edit={startEdit} on:add={() => editMode = 'edit'}/>

      {/if}

    {:else}
      <MeetupDetail id={pageData.id} on:close={closeDetails}/>
    {/if}
  </main>
  