<script lang="ts">
  import '../style.css';

  let count: number = 0;

  function handleClick() {
    count += 1;
  }
</script>

<h1>Hello world!</h1>
<button on:click={handleClick}>Clicked {count} times</button>
