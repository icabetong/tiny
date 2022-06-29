<script lang="ts">
  import { t } from '$lib/translations'
  import { form as useForm, field } from 'svelte-forms'
  import { required, url } from 'svelte-forms/validators'
  import Header from '../components/Header.svelte'
  import Footer from '../components/Footer.svelte'
  import Spinner from '../components/Spinner.svelte'

  const data = field('data', '', [required(), url()])
  const current = useForm(data)
  
  let isWorking = false
  let response: GoTinyResponse = {}
  let error: any | null = null

  async function onCopy() {
    if (response.code) {
      await window.navigator.clipboard.writeText(response.code)
    }
  }

  async function onRequest() {
    isWorking = true
    const hyperlink = $data.value

    try {
      const res = await fetch('https://gotiny.cc/api', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ input: hyperlink })
      })
      const data = await res.json()
      if (data.length > 0) {
        const current = data[0] as GoTinyResponse
        response = current
      }
    } catch (err) {
      error = err
    } finally {
      isWorking = false
    }
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
          placeholder="https://www.example.com"
          disabled={isWorking}
          class="block flex-1 p-2 border rounded-l-md text-md bg-slate-700 border-slate-600 placeholder-gray-400 focus:border-teal-500 focus:outline-none focus:bg-slate-700 active:bg-slate-700"
          aria-required="true"
          aria-disabled={isWorking}/>
        <button 
          disabled={!$current.valid || isWorking}
          type="submit"
          class="bg-teal-500 px-4 py-2 rounded-r-md text-white font-semibold hover:bg-teal-600 focus:outline focus:outline-teal-300 disabled:cursor-normal disabled:opacity-50">
          {$t('common.button.shrink')}
        </button>
      </div>
      {#if isWorking}
        <div class="mt-4">
          <Spinner/>
        </div>
      {/if}
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
            value={`https://www.gotiny.cc/${response.code}`}
            class="appearance-none bg-slate-900 flex-1 text-center outline-none"/>
          <button
            on:click={onCopy}
            type="button"
            class="px-3 py-1 text-sm font-medium rounded-lg bg-slate-700 hover:bg-slate-600 focus:border-teal-500 focus:outline focus:outline-teal-500">
            {$t("common.button.copy")}
          </button>
        </div>
      </div>
    {/if}
    {#if error}
      <div class="w-full px-4 py-4">
        <div class="bg-red-500 w-full p-2 flex flex-row space-x-2 items-center justify-center rounded-lg">
          <p class="text-lg font-medium">{$t('common.feedback.error-generic')}</p>
        </div>
      </div>
    {/if}
  </div>
</main>
<Footer/>