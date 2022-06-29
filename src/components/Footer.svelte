<script lang="ts">
  import { t } from '$lib/translations'
</script>


<footer class="bg-slate-600 text-slate-300 text-sm">
  <div class="max-w-screen-md mx-auto py-4">
    <div class="flex flex-row items-center justify-center space-x-4">
      <a 
        class="link" 
        href="https://gotiny.cc/"
        target="_blank">
        {$t('common.footer.api')}
      </a>
      <a 
        class="link" 
        href="https://github.com/icabetong/tiny"
        target="_blank">
        {$t('common.footer.repo')}
      </a>
    </div>
    <p class="mt-4 text-center text-xs">{$t('common.footer.disclaimer')}</p>
  </div>
</footer>