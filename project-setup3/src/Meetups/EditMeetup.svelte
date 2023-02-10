<script>
    import { createEventDispatcher } from 'svelte'
    import TextInput from "../UI/TextInput.svelte";
    import Button from "../UI/Button.svelte";
    import Modal from '../UI/Modal.svelte';
    import { isEmpty } from '../helpers/validation'


    let title = "";
    let titleValid = false;
    let subtitle = "";
    let address = "";
    let email = "";
    let description = "";
    let imageUrl = "";

    $: titleValid = !isEmpty(title);
    const dispatch = createEventDispatcher();

    function submitForm(){
        dispatch('save', {
            title: title,
            subtitle: subtitle,
            description: description,
            imageUrl: imageUrl,
            contactEmail: email,
            address: address
        });
    }

    function cancel(){
        dispatch('cancel');
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
        <Button type="button" on:click={submitForm}>저장</Button>
        <Button type="button" mode="outline" on:click={cancel}>취소</Button>
      </div>
</Modal>