<script lang="ts">
  import { form as useForm, field } from 'svelte-forms'
  import { required, url } from 'svelte-forms/validators'
  import Navigation from '../components/Navigation.svelte'

  const data = field('data', '', [required(), url()])
  const current = useForm(data)
  
  let response: GoTinyResponse = {}

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

<main class="min-h-screen bg-gradient-to-r from-sky-50 to-teal-50 dark:from-sky-900 dark:to-teal-900 dark:text-white">
  <Navigation/>
  <div class="py-60 max-w-screen-lg">
    
    <div class="h-full flex flex-col items-center justify-center">
      <form 
        on:submit|preventDefault={onRequest}
        class="px-4 py-8 rounded border shadow flex flex-col space-y-4 items-center justify-center bg-white dark:bg-gray-600 dark:border-gray-600">
        <div>
          <label for="url" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">URL</label>
          <input 
            id="url"
            type="text" 
            bind:value={$data.value}
            class="block w-full p-2 text-gray-900 border border-gray-300 rounded bg-gray-50 sm:text-xs focus:ring-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"/>
        </div>
        {#if $current.hasError('data.required')}
          <div>Data is required</div>
        {/if}
      <button 
        type="submit"
        class="bg-teal-500 px-2 py-2 rounded text-white font-semibold">
        Get Response
      </button>
    </form>
    </div>
  </div>
</main>