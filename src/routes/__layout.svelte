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

<slot/>