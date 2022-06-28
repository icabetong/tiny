<script lang="ts">
  import { form as useForm, field } from 'svelte-forms'
  import { required, url } from 'svelte-forms/validators'

  const data = field('data', '', [required(), url()])
  const current = useForm(data)
  
  type Response = {
    code?: string
  }
  let response: Response = {}

  async function onRequest() {
    const hyperlink = $data.value
    
    // const response = await fetch('https://gotiny.cc/api', {
    //   method: 'POST',
    //   headers: { 'Content-Type': 'application/json' },
    //   body: JSON.stringify({ input: hyperlink })
    // })
    setTimeout(() => {
      response = { code: hyperlink }
    }, 3000)
  }
</script>

<main>
  {response.code}
  <form on:submit|preventDefault={onRequest}>
    <input type="text" bind:value={$data.value}/>
    {#if $current.hasError('data.required')}
      <div>Data is required</div>
    {/if}
    <button type="submit">Get Response</button>
  </form>
</main>