<script lang="ts">
  import { t } from '$lib/translations'
</script>


<footer class="bg-slate-600 text-slate-300 text-sm">
  <div class="max-w-screen-md mx-auto py-4">
    <div class="flex flex-row items-center justify-center space-x-4">
      <a href="/">{$t('common.footer.api')}</a>
      <a href="/">{$t('common.footer.repo')}</a>
    </div>
    <p class="mt-4 text-center text-xs">{$t('common.footer.disclaimer')}</p>
  </div>
</footer>