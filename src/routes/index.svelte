<script lang="ts">
  import { t } from '$lib/translations'
  import { form as useForm, field } from 'svelte-forms'
  import { required, url } from 'svelte-forms/validators'
  import { ClipboardCopyIcon } from 'svelte-heroicons-component'
  import Header from '../components/Header.svelte'
  import Footer from '../components/Footer.svelte'

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

<main class="min-h-screen bg-slate-800 text-white">
  <div class="py-32 max-w-screen-sm mx-auto h-full flex flex-col items-center justify-center">
    <Header/>
    <form 
      on:submit|preventDefault={onRequest}
      class="w-full px-4 py-8 flex flex-col items-center justify-center">
      <div class="w-full flex flex-row">
        <input 
          id="url"
          type="text"
          placeholder="https://www.google.com.ph"
          bind:value={$data.value}
          class="peer block flex-1 p-2 border rounded-l-md text-md bg-slate-700 border-slate-600 placeholder-gray-400 focus:ring-teal-500 focus:border-teal-500"/>
          <button 
            type="submit"
            class="bg-teal-500 px-4 py-2 rounded-r-md text-white font-semibold peer-focus:ring-teal-500 peer-focus:border-teal-500">
            {$t('common.button.shrink')}
          </button>
      </div>
      {#if $current.hasError('data.required')}
        <span class="text-sm">Data is required</span>
      {/if}
    </form>
    {#if response.code}
      <div class="w-full px-4 py-4">
        <div class="bg-slate-900 w-full flex flex-row items-center justify-center p-4 rounded-lg">
          <input 
            readonly
            type="text" 
            value={response.code}
            class="appearance-none bg-slate-900 flex-1 text-center outline-none"/>
          <button>
            <ClipboardCopyIcon/>
          </button>
        </div>
      </div>
    {/if}
  </div>
</main>
<Footer/>