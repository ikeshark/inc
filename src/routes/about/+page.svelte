<svelte:head>
  <title>about inc inc inc</title>
</svelte:head>

<!-- where to put show info if and when i get that worked out -->

<main>
  <h2>about</h2>
  <section class="aboutText mt-2">
    <p>music band from <b>baltimore, maryland</b>. rhythm equals pitch.</p>
    <p>the once and future band making anachronistic music &mdash; sometimes atemporal &mdash; with deep-fried electric <b>cello, violin and guitar</b>.</p>
    <p>when they play, anything could happen: forlorn dissonances dance with bubbly polyrhythms, sweet melodies clash with <b>the brutal Monorhythm</b>.</p> 
    <p>beware! <b>crush them under boards with rocks</b>, or risk falling under their siren spell.</p> 
  </section>
  <!-- <p><a target="_blank" rel="noopener noreferrer" href="https://www.instagram.com/incincincmusic/">https://www.instagram.com/incincincmusic/</a></p>
  <p><a target="_blank" rel="noopener noreferrer" href="https://magazine.peabody.jhu.edu/2023/03/21/inc-inc-inc-releases-debut-album/">https://magazine.peabody.jhu.edu/2023/03/21/inc-inc-inc-releases-debut-album/</a></p> -->

  <h2 class="mt-4 leading-tight">get personal with <br> inc inc inc</h2>
  <section class="personalLinks">
    <a target="_blank" rel="noopener noreferrer" href="https://www.rubyfulton.com/">
      👤
      <h2>inc</h2>
    </a>      
    <a target="_blank" rel="noopener noreferrer" href="https://ikeshark.bandcamp.com/">
      👤
      <h2>inc</h2>
    </a>
    <a target="_blank" rel="noopener noreferrer" href="https://soundcloud.com/andrew-histand/whorl">
      👤
      <h2>inc</h2>
    </a>
  </section>
<!-- 
  <h2 class="mt-4">upcoming shows</h2>
  <ul>
    <li>10/16</li>
  </ul> -->

  <h2 class="mt-8">friends of inc</h2>
  <ul class="friends mt-2">
    <li>
      <a target="_blank" rel="noopener noreferrer" href="https://weusedtobefamily.bandcamp.com/">
        we used to be family
        <img height="333" src="https://ikeshark.github.io/portfolio/img/wutbf.jpg" alt="">
      </a>
    </li>
    <li>
      <a target="_blank" rel="noopener noreferrer" href="https://barnyardsharks.bandcamp.com/">
        barnyard sharks
        <img height="333" src="https://ikeshark.github.io/portfolio/img/sharks.jpg" alt="">
      </a>
    </li>
    <li>
      <a target="_blank" rel="noopener noreferrer" href="https://impatiencemachine.bandcamp.com/album/impatience-machine/">
        impatience machine
        <img class="h-333 object-cover" src="https://ikeshark.github.io/portfolio/img/impmach.jpg" alt="">
      </a>
    </li>
    <li>
      <a target="_blank" rel="noopener noreferrer" href="https://ehserecords.bandcamp.com/album/summer-fun">
        nudie suits
        <img height="333" src="https://f4.bcbits.com/img/a3688055488_16.jpg" alt="">
      </a>
    </li>
  </ul>
</main>

<style>
  main {
    padding: 1rem;
  }
  h2 {
    font-size: 1.5rem;
    font-weight: 800;
    text-align: center;
  }
  p {
    margin-bottom: 0.875rem;
    font-size: 1rem;
    line-height: 1.3;
  }
  .aboutText {
    max-width: 666px;
    margin-inline: auto;
  }

  .personalLinks {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin-top: 1.5rem;
  }
  .personalLinks a {
    padding: 0.5rem 1rem;
    text-align: center;
    font-size: 3rem;
    background-color: #e4eecf4b;
    border-radius: 8px;
    box-shadow: 2px 2px 0.33rem rgba(222, 35, 35, 0.285);
  }
  .personalLinks a:hover {
    outline: black 3px solid;
  }
  .personalLinks h2 {
    font-size: 1.75rem;
  }

  .friends {
    display: grid;
    grid-template-columns: 333px;
    padding: 0 1rem;
    gap: 1rem;
    justify-content: center;
    font-weight: 900;
  }
  .h-333 {
    height: 333px;
  }
  @media (min-width: 765px) {
    h2 {
      font-size: 2rem;
    } 
    p {
      font-size: 1.2rem;
    }
    .personalLinks {
      gap: 2rem;
    }
    .personalLinks a {
      padding: 0.75rem 2.5rem;
      font-size: 4rem;
    }
    .personalLinks h2 {
      font-size: 2.5rem;
    }
    .friends {
      grid-template-columns: 333px 333px;
    }
  }
</style>