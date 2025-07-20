<svelte:head>
  <title>inc inc inc</title>
</svelte:head>

<script>
  let incWords = [
    'incorporate',
    'incognito',
    'include',
    'incise',
    'incessant',
    'increment',
    'incantation',
    'incredible',
    'incandescent',
  ]

  function wordJumbler(arr) {
    const bigArr = [...arr, ...arr, ...arr];
    shuffle(bigArr);
    return bigArr;
  }
  // https://stackoverflow.com/questions/59810241/how-to-fisher-yates-shuffle-a-javascript-array
  function shuffle(arr) {
    let i = arr.length;
    let j;
    let temp;
    while(--i > 0) {
      j = Math.floor(Math.random() * (i + 1));
      temp = arr[j];
      arr[j] = arr[i];
      arr[i] = temp;
    }
  }
  let jumbledArr = wordJumbler(incWords);

</script>


<main>
  {#each jumbledArr as incWord}
    <span>{incWord}</span>
  {/each}
</main>
<img src="https://f4.bcbits.com/img/0031589850_100.png" alt="">

<style>
  img {
    width: 100vw;
    filter: contrast(130%); 
    /* opacity: .8; */
    position: fixed;
    bottom: 0;
  }
  main {
    padding: 1rem;
    padding-top: 2rem;
    max-width: 44rem;
    margin-inline: auto;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;

  }
  main span {
    font-size: 0.8rem;
    line-height: 1;
    margin-bottom: 1rem;
  }
  main span:nth-child(3n) {
    text-align: right;
  }
  main span:nth-child(3n - 1) {
    text-align: center;
  }
  @media (min-width: 768px) {
    main {
      padding: 2rem;
      padding-top: 3rem;
    }
    main span {
      font-size: 1.15rem;
      line-height: 1.2;
      letter-spacing: .325rem;
    }
  } 
</style>