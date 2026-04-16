<script>
  let {header, children, footer} = $props();
</script>

<div class="card-frame">
  <header class="card-header">
    <!-- @render props이름()  -->
    {@render header()}
  </header>
  <main class="card-body">
    {@render children()}
  </main>
  <footer class="card-footer">
    {#if footer}
      {@render footer()}
    {:else}
      <button>기본 확인</button>
    {/if}
  </footer>
</div>

<style>
  .card-frame{border: 2px solid #333; border-radius: 10px; overflow: hidden;}
  .card-header{background: #f4f4f4; padding: 10px; border-bottom:  1px solid #ddd;}
  .card-body{padding: 20px;}
  .card-footer {background: #eee; padding: 10px; text-align: right;}
</style>