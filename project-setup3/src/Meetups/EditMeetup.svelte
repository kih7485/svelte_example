<script>
    import meetups from './meetups-sotre';
    import { createEventDispatcher } from 'svelte'
    import TextInput from "../UI/TextInput.svelte";
    import Button from "../UI/Button.svelte";
    import Modal from '../UI/Modal.svelte';
    import { isEmpty } from '../helpers/validation'

    export let id = null;

    let title = "";
    let subtitle = "";
    let address = "";
    let email = "";
    let description = "";
    let imageUrl = "";

    let formIsValid = false;


    if(id){
      const unSubscribe = meetups.subscribe(items => {
        const selectedMeetup = items.find(item => item.id === id);
        title = selectedMeetup.title;
        subtitle = selectedMeetup.subtitle;
        address = selectedMeetup.address;
        email = selectedMeetup.email;
        description = selectedMeetup.description;
        imageUrl = selectedMeetup.imageUrl;
      });

      unSubscribe();
    }


    $: titleValid = !isEmpty(title);
    $: subtitleValid = !isEmpty(subtitle);
    $: formIsValid = titleValid && subtitleValid;
    const dispatch = createEventDispatcher();

    function submitForm(){
        if(!formIsValid){
            return;
        }
        const meetupData = {
            title: title,
            subtitle: subtitle,
            description: description,
            imageUrl: imageUrl,
            contactEmail: email,
            address: address
        };
  
      // meetups.push(newMeetup); // DOES NOT WORK!
      
      if(id){
        fetch(`https://svelte-course-214ac-default-rtdb.firebaseio.com/meetups/${id}.json`, {
          method: 'PATCH',
          body: JSON.stringify({...meetupData}),
          headers: {'Content-Type': 'application/json'}
        })
        .then(res => {
          if(!res.ok){
            throw new Error("실패");
          }

          meetups.updatedMeetup(id, meetupData);
        })
        .catch(err => console.error(err))
        
      }else{
        fetch("https://svelte-course-214ac-default-rtdb.firebaseio.com/meetups.json", {
          method: 'POST',
          body: JSON.stringify({...meetupData, isFavorite: false}),
          headers: {'Content-Type': 'application/json'}
        })
        .then(res => {
          if(!res.ok){
            throw new Error("실패");
          }

          return res.json();
        })
        .then(data => {
          meetups.addMeetup({...meetupData, isFavorite:false, id: data.name});
          console.log(data)
        })
        .catch(err => console.error(err))
       
      }
      dispatch('save');
    }

    function cancel(){
      dispatch('cancel');
    }

    function deleteMeetup(){
      fetch(`https://svelte-course-214ac-default-rtdb.firebaseio.com/meetups/${id}.json`, {
          method: 'DELETE'
        })
        .then(res => {
          if(!res.ok){
            throw new Error("실패");
          }
          meetups.removeMeetup(id);
        })
        .catch(err => console.error(err))

      
      dispatch('save');
    }
</script>

<style>

form {
      width: 100%;
    }
</style>

<Modal title="edit meetup data - {title}" on:cancel> 
    <form on:submit|preventDefault={submitForm}>
        <TextInput
          id="title"
          label="Title"
          valid={titleValid}
          validityMessage="유효한 제목을 입력하세요"
          value={title}
          on:input={event => (title = event.target.value)} />
        <TextInput
          id="subtitle"
          label="Subtitle"
          valid={subtitleValid}
          validityMessage="소제목을 입력하세요"
          value={subtitle}
          on:input={event => (subtitle = event.target.value)} />
        <TextInput
          id="address"
          label="Address"
          value={address}
          on:input={event => (address = event.target.value)} />
        <TextInput
          id="imageUrl"
          label="Image URL"
          value={imageUrl}
          on:input={event => (imageUrl = event.target.value)} />
        <TextInput
          id="email"
          label="E-Mail"
          type="email"
          value={email}
          on:input={event => (email = event.target.value)} />
        <TextInput
          id="description"
          label="Description"
          controlType="textarea"
          value={description}
          on:input={event => (description = event.target.value)} />
      </form>
      <div slot="footer">
        <Button type="button" on:click={submitForm} disabled={!formIsValid}>저장</Button>
        <Button type="button" mode="outline" on:click={cancel}>취소</Button>
        {#if id}
          <Button type="button" on:click={deleteMeetup}>삭제</Button>
        {/if}
      </div>
</Modal>