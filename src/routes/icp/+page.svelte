<svelte:head>
  <title>ice cream in panama</title>
</svelte:head>

<script>
  import { onMount } from 'svelte';
  import { base } from '$app/paths';

  import * as Tone from 'tone';
  import { loops } from '$lib'

  let iceCreamGtr;
  let iceCreamVln;
  let iceCreamVlc;

  let beachGtr;
  let beachVln;
  let beachVlc;

  let sharkGtr;
  let sharkVln;
  let sharkVlc;

  let volcanoGtr;
  let volcanoVln;
  let volcanoVlc;

  let snorkelGtr;
  let snorkelVln;
  let snorkelVlc;

  let delay;
  let delay2;
  let pitchShifter1;
  let pitchShifter2;

  let toneLoops = {}
  let selectedLoopIndex = $state(null);
  let timeline = $state([]);
  let lengthInput = $state('');
  let isPlaying = $state(false);
  let playingIndex = $state(null);

  onMount(() => {
    iceCreamGtr = new Tone.Player(`${base}/samples/icp_gtr.mp3`).toDestination();
    iceCreamVln = new Tone.Player(`${base}/samples/icp_vln.mp3`).toDestination();
    iceCreamVlc = new Tone.Player(`${base}/samples/icp_vlc.mp3`).toDestination();

    beachGtr = new Tone.Player(`${base}/samples/beach_gtr.mp3`).toDestination();
    beachVln = new Tone.Player(`${base}/samples/beach_vln.mp3`).toDestination();
    beachVlc = new Tone.Player(`${base}/samples/beach_vlc.mp3`).toDestination();

    sharkGtr = new Tone.Player(`${base}/samples/shark_gtr3.mp3`).toDestination();
    sharkVln = new Tone.Player(`${base}/samples/shark_vln3.mp3`).toDestination();
    sharkVlc = new Tone.Player(`${base}/samples/shark_vlc2.mp3`).toDestination();

    volcanoGtr = new Tone.Player(`${base}/samples/volcano_gtr2.mp3`).toDestination();
    volcanoVln = new Tone.Player(`${base}/samples/volcano_vln2.mp3`).toDestination();
    volcanoVlc = new Tone.Player(`${base}/samples/volcano_vlc2.mp3`).toDestination();

    snorkelGtr = new Tone.Player(`${base}/samples/snorkel_gtr2.mp3`).toDestination();
    snorkelVln = new Tone.Player(`${base}/samples/snorkel_vln.mp3`).toDestination();
    snorkelVlc = new Tone.Player(`${base}/samples/snorkel_vlc2.mp3`).toDestination();

    toneLoops = {
      iceCreamGtr,
      iceCreamVln,
      iceCreamVlc,
      
      beachGtr,
      beachVln,
      beachVlc,

      sharkGtr,
      sharkVln,
      sharkVlc,

      volcanoGtr,
      volcanoVln,
      volcanoVlc,

      snorkelGtr,
      snorkelVln,
      snorkelVlc,
    }

    Object.values(toneLoops).forEach(loop => loop.loop = true)

    pitchShifter1 = new Tone.PitchShift(12).toDestination();
    pitchShifter2 = new Tone.PitchShift(-12).toDestination();

    delay = new Tone.Delay('2n').toDestination();
    delay2 = new Tone.Delay('4n').toDestination();

    Tone.getTransport().bpm.value = 186;
  });
    
  function play(timelineArr) {
    // before was calling stop() at beginning of play. but that complicates having 'preview' and 'play' modes
    Tone.getTransport().stop();

    let curMeasure = 0;

    const totalMeasures = timelineArr.reduce((acc, cur) => {
      return (cur.measures * cur.reps) + acc
    }, 0)

    timelineArr.forEach((loop, i) => {
      const { reps, measures, defaultMeasures, variant, name } = loop;
  
      const lengthInSec = Tone.Transport.toSeconds(`${measures * reps}m`);

      const gtr = toneLoops[name + 'Gtr'];
      const vln = toneLoops[name + 'Vln'];
      const vlc = toneLoops[name + 'Vlc'];

      const prevLoopType = timelineArr[i - 1]?.name;

      Tone.getTransport().scheduleOnce(time => { 
        playingIndex = i;

        vlc.playbackRate = 1
        vln.playbackRate = 1
        vln.disconnect();
        vlc.disconnect();
        gtr.disconnect();

        gtr.toDestination();

        if (variant === 'Alt2') {
          vln.connect(delay)
          vlc.connect(delay2)
        } else if (variant === 'Alt1') {
          vlc.playbackRate = 0.5;
          vln.playbackRate = 2;
          vlc.connect(pitchShifter1)
          vln.connect(pitchShifter2)
        } else {
          vln.toDestination();
          vlc.toDestination();
        }

        // if prev loop is the same type of loop
        if (prevLoopType === name) {
          gtr.restart(time, 0, `${measures * reps}m`);
          vln.restart(time, 0, lengthInSec * vln.playbackRate);
          vlc.restart(time, 0, lengthInSec * vlc.playbackRate);
        } else {
          gtr.start(time, 0, `${measures * reps}m`);
          vln.start(time, 0, `${measures * reps  * vln.playbackRate}m`);
          vlc.start(time, 0, `${measures * reps  * vlc.playbackRate}m`);
        }

        if (measures !== defaultMeasures) {
          gtr.loopEnd = `${measures}m`;
          vln.loopEnd = `${measures}m`;
          vlc.loopEnd = `${measures}m`;
        } else {
          gtr.loopEnd = 0;
          vln.loopEnd = 0;
          vlc.loopEnd = 0;
        }


      }, `${curMeasure}:0:0`);
      
      curMeasure += measures * reps;      
    });

    // https://github.com/Tonejs/Tone.js/wiki/Performance
    Tone.getTransport().start('+0.1'); 
    Tone.Master.mute = false;

    Tone.getTransport().scheduleOnce(time => { 
      isPlaying = false;
      playingIndex = null;

    },`${totalMeasures}:0:0`);
  }
  function stop() {
    isPlaying = false;
    Tone.Master.mute = true;
    Tone.getTransport().stop();
    Tone.getTransport().cancel();
  }

  function schedule(e) {
    const value = e.target.value;
    const obj = { ...loops[value]};
    timeline.push(obj)
  }
  function selectLoop(e) {
    const value = e.currentTarget.getAttribute('data-index');

    lengthInput = timeline[parseInt(value)].measures;

    selectedLoopIndex = parseInt(value)
  }
  function changeLength(e) {
    lengthInput = e.target.value;
    timeline[selectedLoopIndex].measures = parseInt(lengthInput);
  }
  function previewLoop(e) {
    if (isPlaying) {
      stop();
    } else {
      isPlaying = 'preview';

      const tempTimeline = [timeline[selectedLoopIndex]]
      play(tempTimeline)
    }
  }
  function playBtn(e) {
    isPlaying = 'play';
    selectedLoopIndex = null;

    play([...timeline])
  }
  function scheduleExpand({ target }) {
    timeline[selectedLoopIndex].variant = target.checked ? 'Alt1' : '';
  }
  function scheduleDelay({ target }) {
    timeline[selectedLoopIndex].variant = target.checked ? 'Alt2' : '';
  }
  function repeat(e) {
    const { value } = e.target;
    const num = parseInt(value);

    if (isNaN(num)) {
      timeline[selectedLoopIndex].reps = 1
    } else {
      timeline[selectedLoopIndex].reps = num
    }
  }
  function deleteItem() {
    timeline = timeline.filter((item, i) => i !== selectedLoopIndex);
    selectedLoopIndex = null;
  }

  // bounce samples quieter, and better fades/cuts, quantize around 1/2n marks
  // https://github.com/metonym/sveltekit-gh-pages/blob/master/README.md
  // https://github.com/khromov/derivault/blob/main/.github/workflows/build.yml
  // https://www.reddit.com/r/sveltejs/comments/synqd4/sveltekit_relative_paths/
  // RUBY IDEA: selected emoji also becomes background (replacing black bg)
</script>


  <main>
    <section class="mt-4" id="samples">
      <h2>samples</h2>
      <button disabled={isPlaying} onclick={schedule} value="iceCream">🍦</button>
      <button disabled={isPlaying} onclick={schedule} value="beach">🏝️</button>
      <button disabled={isPlaying} onclick={schedule} value="shark">🦈</button>
      <br>
      <button disabled={isPlaying} onclick={schedule} value="volcano">🌋</button>
      <button disabled={isPlaying} onclick={schedule} value="snorkel">🤿</button>
    </section>

    <section class="mt-4" id="effects">
      <h2>effects</h2>
      {#if selectedLoopIndex || selectedLoopIndex === 0}

      <div id="effectsGrid">
        <button 
          class={isPlaying === 'preview' ? `w${timeline[selectedLoopIndex].measures} bg-white playing` : `w${timeline[selectedLoopIndex].measures} bg-white`} 
          id="preview" 
          onclick={previewLoop}
          data-variant={timeline[selectedLoopIndex].variant}
        >
          {timeline[selectedLoopIndex].icon}
        </button>
   
        <section id="variants" class="p-1 border-black border-solid border-2 bg-white rounded-sm">
          <h3 class="font-black text-xl">variants</h3>
          <label class={timeline[selectedLoopIndex].variant === 'Alt2' ? 'selected variants delay' : 'variants delay'}>
            <input 
              class="sr-only"
              onchange={scheduleDelay}
              type="checkbox" 
              checked={timeline[selectedLoopIndex].variant === 'Alt2'}
            >
              delay
          </label>

          <label class={timeline[selectedLoopIndex].variant === 'Alt1' ? 'selected variants expand' : 'variants expand'}>
            <input 
              class="sr-only"
              onchange={scheduleExpand} 
              type="checkbox" 
              checked={timeline[selectedLoopIndex].variant === 'Alt1'}
            >
              expand
          </label>
        </section>
        
        <label id="length" class="w-full p-1 border-black border-solid border-2 bg-white rounded-sm">
          <b class="font-black text-xl">length</b>
          <input 
            class="block mt-4 w-full"
            type="range" 
            min="1" max={timeline[selectedLoopIndex].defaultMeasures} 
            step="1"
            value={lengthInput} 
            onchange={changeLength}
          >
          {lengthInput}
        </label>

        <label id="reps" class="w-full p-2 border-black border-solid border-2 bg-white rounded-sm">
          <b class="font-black text-xl block">reps</b>
          <input 
            class="block mt-4 w-full"
            type="range" 
            min="1"
            max="8"
            step="1"
            value={timeline[selectedLoopIndex].reps} 
            onchange={repeat}
          >
          {timeline[selectedLoopIndex].reps}
        </label>

        <button id="deleteBtn" onclick={deleteItem}>delete</button>
      </div>
    
      {:else}
      <p class="mt-4"><i>please select a loop on the timeline</i></p>
      {/if}
    </section>

    <section class="mt-6" id="timeline">
      <h2>timeline</h2>
      <ul id="timelineGrid">
        {#each timeline as item, i}
          <li data-reps={item.reps} class={`w${item.measures}`}>
            <button 
              class={i === selectedLoopIndex ? 'selected' : i === playingIndex ? 'playing' : '' } 
              data-index={`${i}`} 
              data-variant={item.variant}
              value={item.icon}
              onclick={selectLoop}
              disabled={isPlaying}
            >
              <span class={`block wid${item.measures}`}>{item.icon}</span>
            </button>
          </li>
        {/each}
      </ul>
    </section>

    <section class="mt-4" id="controls">
      <button id="play" disabled={isPlaying || !timeline.length} onclick={playBtn}>play</button>
      <button id="stop" disabled={!isPlaying} onclick={stop}>stop</button>
    </section>
  </main>


  <style>
    main {
      padding: 1rem 1rem 4rem;
      background-color: rgb(33, 33, 33);
      position: relative;

      text-align: center;

      display: grid;
      gap: 0.75rem 1.5rem;
      grid-template-columns: 1fr;
      justify-content: center;
      grid-template-areas: 
        'samples' 
        'effects'
        'timeline'
        'controls';
    }
    @media (min-width: 768px) {
      main {
        grid-template-columns: min(40vw, 666px) min(40vw, 666px);
        grid-template-areas: 
          'samples effects'
          'timeline timeline'
          'controls controls';
      }      
    }
    h2 {
      background-color: #fff;
      border-radius: 3px;
      font-size: 1.4rem;
      font-weight: 900;
      left: 50%;
      line-height: 1.2;
      margin-inline: auto;
      padding: 0.125rem 0.5rem 0.25rem;
      box-shadow: 1px 1px 1rem 1px rgba(173, 216, 230, 0.669);
      position: absolute;
      top: -1.325rem;
      transform: translateX(-50%);
      width: fit-content;
    }
    section {
      position: relative;
      background-color: #fff;
      outline: rgba(255, 255, 255, 0.85) 4px solid;
      border-radius: 3px;
    }

    #samples {
      grid-area: samples;
      border: 4px solid rgb(35, 127, 158);
      padding: 1rem 3.5rem 1rem 1.5rem;
      background-image: radial-gradient(transparent 50%, rgba(45, 146, 180, 0.468), rgb(45, 146, 180));
    }
    #samples button {
      font-size: 4rem;
    }
    @media (min-width: 768px) {
      #samples button {
        font-size: 7rem;
      }
    }
    #effects {
      grid-area: effects;
      border: 4px solid orange;
      padding: 0.5rem;
      background-image: radial-gradient(transparent 50%, rgba(255, 166, 0, 0.329), rgb(228, 148, 0));
    }
    #effectsGrid {
      display: grid;
      height: 100%;
      padding-top: 1rem;
      place-items: center;
      gap: 0.5rem;
      grid-template-rows: 1fr 1fr;
      grid-template-areas: 
        'preview variants'
        'preview length'
        'delete reps';
    }
    #deleteBtn {grid-area: delete}
    #reps {grid-area: reps}
    #length {grid-area: length}
    #variants {grid-area: variants}

    #deleteBtn {
      background-color: #c00;
      color: white;
    }

    #effects button {
      font-size: 1rem;
      margin: 1rem auto;
      border: 2px solid black;
      padding: 0.3rem 1rem;
      display: block;
      border-radius: 5px;
      font-weight: 600;
    }
    #effects #preview {
      font-size: 3.5rem;
      padding: 0.1rem;
      grid-area: preview;
      overflow: hidden;
      position: relative;

    }
    #preview::after {
      inset: 0;
      left: 1rem;
      top: 25%;
      content: '';
      position: absolute;
      background-color: rgba(0, 0, 0, 0.613);
      height: 50%;
      clip-path: polygon(0 0, 0% 100%, 75% 50%);
    }
    #preview.playing::after {
      clip-path: unset;
      right: 1rem;

    }
    @media (min-width: 768px) {
      #effects #preview {
        font-size: 6.5rem;
      }
    }

    .variants {
      padding: 0.3rem 1rem;
      border: 2px solid rgb(0, 221, 255);
      font-weight: 600;
      margin-top: 0.5rem;
      display: inline-block;
      border-radius: 8px;
      margin-inline: 1px;
    }
    .variants.selected {
      border-color: black;
      display: inline-block; 
      background-color: #efefef;
    }


    #timeline {
      border: 4px solid rgb(11, 165, 11);
      padding: 1rem 0.25rem 0.25rem;
      width: 100%;
      grid-area: timeline;
      min-height: 8.5rem;
    }
    #timelineGrid {
      font-size: 4rem;
      display: grid;
      gap: 0.5rem;
      grid-template-columns: repeat(auto-fill, 1ch);
      gap: 0rem;  
    }
    #timelineGrid li {
      border: 1px solid gray;
    }
    #timelineGrid button {
      display: block;
      position: relative;
    }
    #timelineGrid button span {
      overflow: hidden;
    }
    #timeline .selected {
      outline: 4px solid purple
    }
    #timeline .playing {
      outline: 4px solid rgb(128, 0, 30)
    }

    #controls {
      grid-area: controls;
      width: fit-content;
      margin-inline: auto;
      padding: 1rem;
    }

    #stop, #play {
      font-weight: 900;
      border: 2px solid gray;
      border-radius: 5px;
      padding: 0.25rem 1rem;
      display: inline-block;
      width: fit-content;
    }
    #stop:disabled, #play:disabled {
      background-color: #dfdfdf;
    }

    [data-variant='Alt1'] {
      background: pink;
    }
    [data-variant='Alt2'] {
      background: lightblue;
    }
    [data-reps='1'] {--reps: 1}
    [data-reps='2'] {--reps: 2}
    [data-reps='3'] {--reps: 3}
    [data-reps='4'] {--reps: 4}
    [data-reps='5'] {--reps: 5}
    [data-reps='6'] {--reps: 6}
    [data-reps='7'] {--reps: 7}
    [data-reps='8'] {--reps: 8}

    [data-reps='2'] button span {
      filter: drop-shadow(1ch 0 0 red);
    }
    [data-reps='3'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange);
    }
    [data-reps='4'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange)
        drop-shadow(1ch 0 0 yellow)
    }
    [data-reps='5'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange)
        drop-shadow(1ch 0 0 yellow)
        drop-shadow(1ch 0 0 orange);
    }
    [data-reps='6'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange)
        drop-shadow(1ch 0 0 yellow)
        drop-shadow(1ch 0 0 green)
        drop-shadow(1ch 0 0 blue);
    }
    [data-reps='7'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange)
        drop-shadow(1ch 0 0 yellow)
        drop-shadow(1ch 0 0 green)
        drop-shadow(1ch 0 0 blue)
        drop-shadow(1ch 0 0 indigo);

    }
    [data-reps='8'] button span {
      filter: 
        drop-shadow(1ch 0 0 red) 
        drop-shadow(1ch 0 0 orange)
        drop-shadow(1ch 0 0 yellow)
        drop-shadow(1ch 0 0 green)
        drop-shadow(1ch 0 0 blue)
        drop-shadow(1ch 0 0 indigo)
        drop-shadow(1ch 0 0 rgb(243, 0, 243));

      }    
    /* [data-reps='2'] button::after {
      content: attr(value);
      position: absolute;
      top: 0;
      right: -90%;
      /* filter: drop-shadow(1ch 0px 1px rgba(0, 0, 0, 0.191)); */
      /* use grid instead of flex and have col span x or whatever 
    } */

    [data-reps='3'] {
      --reps: 3
    }
    .wid1 { width: 1ch; }
    .wid2 { width: 2ch; }
    .wid3 { width: 3ch; }
    .wid4 { width: 4ch; }
    .wid5 { width: 5ch; }
    .wid6 { width: 6ch; }
    .wid7 { width: 7ch; }
    .wid8 { width: 8ch; }
    .wid9 { width: 9ch; }

    .w1 { grid-column: span calc(1 * var(--reps)); }
    .w2 { grid-column: span calc(2 * var(--reps)); }
    .w3 { grid-column: span calc(3 * var(--reps)); }
    .w4 { grid-column: span calc(4 * var(--reps)); }
    .w5 { grid-column: span calc(5 * var(--reps)); }
    .w6 { grid-column: span calc(6 * var(--reps)); }
    .w7 { grid-column: span calc(7 * var(--reps)); }
    .w8 { grid-column: span calc(8 * var(--reps)); }
    .w9 { grid-column: span calc(9 * var(--reps)); }

    .selected.delay {
      text-shadow: 8px -6px rgba(0,0,0,0.15);
    }
    .expand {
      position: relative;
    }
    .selected.expand::before {
      content: 'expand';
      position: absolute;
      color: rgba(0,0,0,0.125);
      width: 100%;
      height: 100%;
      left: 70%;
      transform: scale(1.4) translateX(-50%);
    }
  </style>