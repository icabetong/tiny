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

  async function onCopy() {
    if (response.code) {
      await window.navigator.clipboard.writeText(response.code)
    }
  }

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

<svelte:head>
  <title>{$t("common.app-name")}</title>
  <meta name="description" content={$t('common.desc')}/>
</svelte:head>

<main class="min-h-screen bg-slate-800 text-white">
  <div class="py-32 max-w-screen-sm mx-auto h-full flex flex-col items-center justify-center">
    <Header/>
    <form 
      on:submit|preventDefault={onRequest}
      class="w-full px-4 py-8 flex flex-col items-center justify-center">
      <label for="url" class="w-full px-1 py-2 text-sm text-slate-300">{$t('common.field.url')}</label>
      <div class="w-full flex flex-row">
        <input 
          bind:value={$data.value}
          required
          id="url"
          type="text"
          placeholder="https://www.google.com.ph"
          class="block flex-1 p-2 border rounded-l-md text-md bg-slate-700 border-slate-600 placeholder-gray-400 focus:border-teal-500 focus:outline-none focus:bg-slate-700 active:bg-slate-700"
          aria-required="true"/>
        <button 
          disabled={!$current.valid }
          type="submit"
          class="bg-teal-500 px-4 py-2 rounded-r-md text-white font-semibold focus:outline focus:outline-cyan-300 disabled:cursor-normal disabled:opacity-50">
          {$t('common.button.shrink')}
        </button>
      </div>
      {#if $current.hasError('data.url')}
        <span class="w-full text-sm px-1 py-2 text-red-500">{$t('common.feedback.input-not-url')}</span>
      {/if}
    </form>
    {#if response.code}
      <div class="w-full px-4 py-4">
        <div class="bg-slate-900 w-full p-2 flex flex-row items-center justify-center rounded-lg">
          <input 
            readonly
            type="text" 
            value={response.code}
            class="appearance-none bg-slate-900 flex-1 text-center outline-none"/>
          <button
            on:click={onCopy}
            type="button"
            class="p-2 rounded-full hover:bg-slate-700 focus:border-cyan-500 focus:outline focus:outline-cyan-500">
            <ClipboardCopyIcon/>
          </button>
        </div>
      </div>
    {/if}
  </div>
</main>
<Footer/>