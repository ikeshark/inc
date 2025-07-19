<script>
  import { base } from '$app/paths';

  let ftbotwv1Src = 'https://bandcamp.com/EmbeddedPlayer/album=109934235/size=large/bgcol=ffffff/linkcol=0687f5/tracklist=true/transparent=true/'
  let iiiSrc = 'https://bandcamp.com/EmbeddedPlayer/album=3761951308/size=large/bgcol=f4f4f4/linkcol=0687f5/tracklist=true/transparent=true/'
</script>

<main>
  <iframe title="From the bottom of the world music player" src={ftbotwv1Src} seamless>
    <a href="https://incincinc.bandcamp.com/album/from-the-bottom-of-the-world-vol-1">From the Bottom of the World, Vol 1 by Inc Inc Inc</a>
  </iframe>
  <section class="p-4 space-y-4 max-w-prose">
    <h2 class="font-black text-xl">from the bottom of the world, vol 1</h2>
    <p>recorded in the dark depths, past the swamp of stale ass mouth smell</p>
    <p>illuminated by gallium peppering the corners of the walls</p>
    <p>three smartphones, a cello, violin, guitar, and five oblique strategies</p>
    <p>sound collage, lo-fi, effects pedals, free improv</p>
  </section>
  <iframe title="inc inc inc music player" src={iiiSrc} seamless>
    <a href="https://incincinc.bandcamp.com/album/inc-inc-inc">inc inc inc</a>
  </iframe>
  <section class="p-4 space-y-4 max-w-prose">
    <h2 class="font-black text-xl">inc inc inc</h2>
    <p>a challenge of the highest order during the shortest month</p>
    <p>the reptilian bistro was more than gracious hosting inc<sup>3</sup> as we battled time with rhythm and pitch</p>
    <p>melancholic folk, polyrhythmic rock, cellular madness melting into the fracturing of time itself</p>
    <p>make your own <a class="font-semibold underline text-blue-700" href="{base}/icp">ice cream in panama</a></p>
  </section>
</main>

<style>

  main {
    --width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    border: 1rem solid white;
    gap: 0.5rem;
  }
  iframe {
    width: var(--width);
    height: 475px;
  }
  @media (min-width: 555px) {
    main {
      --width: 333px;
      gap: 1rem 0.5rem;
      grid-template-columns: var(--width) 1fr;
    }
    iframe {
      height: 420px;
    }
  }
  @media (min-width: 1210px) {
    main {
      grid-template-columns: var(--width) 1fr var(--width) 1fr;
    }
    iframe {
      height: 630px;
    }
  }
</style>

<!-- <iframe style="border: 0; width: 350px; height: 470px;" src="https://bandcamp.com/EmbeddedPlayer/album=3761951308/size=large/bgcol=ffffff/linkcol=0687f5/tracklist=false/transparent=true/" seamless><a href="https://incincinc.bandcamp.com/album/inc-inc-inc">Inc Inc Inc by Inc Inc Inc</a></iframe> -->