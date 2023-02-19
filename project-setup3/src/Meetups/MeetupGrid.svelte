<script>
    import { createEventDispatcher } from 'svelte'
    import Button from "../UI/Button.svelte";
    import MeetupFilter from "./MeetupFilter.svelte";
    import MeetupItem from "./MeetupItem.svelte";
    import {scale} from 'svelte/transition'
    import { flip } from 'svelte/animate';
  
    export let meetups;

    const dispatch = createEventDispatcher();
    let favsOnly = false;

    $: filterMeetups = favsOnly ? meetups.filter(item => item.isFavorite) : meetups;

    function setFilter(event){
      console.log(event.detail)
      favsOnly = event.detail === 1;
    }
</script>
  
  <style>
    section {
      width: 100%;
      display: grid;
      grid-template-columns: 1fr;
      grid-gap: 1rem;
    }
  
    @media (min-width: 768px) {
      section {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    #meetup-controls{
      margin: 1rem;
      display: flex;
      justify-content: space-between;
    }
  </style>
  
  <section id="meetup-controls">
    <Button on:click={() => {dispatch('add')}}>
      New Meetup
    </Button>
    <MeetupFilter on:select={setFilter}/>

  </section>
  <section id="meetups">
    {#each filterMeetups as meetup (meetup.id)}
      <div trasition:scale animate:flip={{duration:300}}> 
        <MeetupItem 
          id={meetup.id}
          title={meetup.title}
          subtitle={meetup.subtitle}
          description={meetup.description}
          imageUrl={meetup.imageUrl}
          email={meetup.contactEmail}
          address={meetup.address}
          isFav={meetup.isFavorite}
          on:showdetails
          on:edit
        />
      </div>
    {/each}
  </section>
  