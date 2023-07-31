<script lang="ts">
    import { onMount } from 'svelte';
  
    export let konami = false;
  
    let combo: string[] = [];
    let time = Date.now();
  
    function combinator(event: KeyboardEvent) {
      const list = ['ArrowUp', 'ArrowUp', 'ArrowDown', 'ArrowDown', 'ArrowLeft', 'ArrowRight', 'ArrowLeft', 'ArrowRight', 'b', 'a', 'Enter'];
      const now = Date.now();
  
      if (!list.includes(event.key)) return;
      if (now - time > 4000) combo = [];
  
      combo.push(event.key);
      time = now;
      console.log(combo);
      if (JSON.stringify(combo) === JSON.stringify(list)) {
        konami = !konami;
      }
    }
    onMount(() => {
      window.addEventListener('keyup', combinator);
  
      return () => {
        window.removeEventListener('keyup', combinator);
      };
    });
  </script>
  
  <svelte:window />
  