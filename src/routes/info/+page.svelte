<script>
	import { goto } from "$app/navigation";
	import { resolve } from "$app/paths";
	import BaseCard from "./BaseCard.svelte";
  
</script>


<BaseCard>
  {#snippet header()}
    <h3>정보</h3>
  {/snippet}
  <p>문의 : dzface031@gmail.com </p>
  <p>프로젝트 구성 : 스벨트킷 + EmulatorJs </p>
  {#snippet footer()}
    <button style="color: red;" onclick={()=> goto(resolve("/"))}>닫기</button>
  {/snippet}
</BaseCard>