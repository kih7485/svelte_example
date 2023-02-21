<script>
    import Modal from "./Modal.svelte";

    export let message;
</script>

<Modal title="에러 발생..." on:cancel>
    <p>{message}</p>
</Modal>