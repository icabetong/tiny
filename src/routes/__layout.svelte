<script context="module">
  import '../app.css'
  import { locale, loadTranslations } from '$lib/translations'

  /** @type {import('@sveltejs/kit').Load} */
  export const load = async ({ url }) => {
    const { pathname } = url
    const defaultLocale = 'en'
    const initLocale = locale.get() || defaultLocale

    await loadTranslations(initLocale, pathname)

    return {}
  }

</script>

<svelte:head>  
  <meta name="keywords" content="GoTiny, URL Shortener, Tiny, URL"/>
  <meta name="author" content="Isaiah Collins Abetong"/>
  <meta name="robots" content="all"/>
  <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale=1" />
	<meta name="apple-mobile-web-app-capable" content="yes"/>

  <meta property="og:locale" content="en_US"/>
  <meta property='og:title' content="Tiny - the URL Shortener"/>
	<meta property='og:url' content="http://www.github.com/icabetong/tiny"/>
	<meta property='og:site_name' content="Tiny"/>
	<meta property='og:type' content='website'/>
</svelte:head>

<slot/>