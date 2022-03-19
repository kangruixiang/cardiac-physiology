<script>
  let LB = 140;
  let inches = 70;
  let HGB = 14;
  let Age = 70;
  let SaO2 = 100;
  let SvO2 = 70;
  let VO2;
  let BSA;
  let MAP = 93;
  let CVP = 6;
  let feet;
  let CM;
  let KG;
  let feetRemainder;
  $: feet = inches / 12;
  $: CM = inches * 2.54;
  $: KG = LB * 0.45359237;
  $: feetRemainder = inches % 12;
  $: BSA = Math.sqrt((inches * 2.54 * LB * 0.45359237) / 3600);
  $: Age >= 70 ? (VO2 = 110 * BSA) : (VO2 = 125 * BSA);
  $: CO = VO2 / (((SaO2 - SvO2) / 100) * HGB * 13.4);
  $: CI = CO / BSA;
  $: SVR = (80 * (MAP - CVP)) / CO;
</script>

<main>
  <div class="container flex align-middle justify-center max-w-4xl">
    <div
      class="py-4 px-6 my-6 rounded-lg lg:p-24 w-full lg:border-2 lg:border-solid lg:border-zinc-900 xl:max-w-6xl"
    >
      <div class="border-zinc-400 border-b mb-4 py-4">
        <h2>Cardiac Physiology</h2>
        <div class="flex w-full">
          Cardiac Output (4.0-8.0 L/min):&nbsp;
          <span class="font-semibold" class:warning={CO < 4}
            >{Math.round(CO * 100) / 100}</span
          >
        </div>
        <div class="flex w-full">
          Cardiac Index (2.0 - 4.0 L/min/m2):&nbsp;
          <span class="font-semibold" class:warning={CI < 2}
            >{Math.round(CI * 100) / 100}</span
          >
        </div>
        <div class="flex w-full">
          SVR (900 - 1440 dyn/s/cm-5):&nbsp;
          <span class="font-semibold" class:warning={SVR < 900 || SVR > 1440}
            >{Math.round(SVR)}</span
          >
        </div>
      </div>

      <div id="bottom" class="flex flex-col">
        <div>
          <label class="label">Weight (lbs)</label>
          <div class="line">
            <input
              type="range"
              class="w-20"
              bind:value={LB}
              min="80"
              max="300"
            />
            <span class="w-12">{Math.round(KG)} kg</span>
            <input type="text" class="input-text" bind:value={LB} />
          </div>
        </div>
        <div>
          <label class="label">Height (in)</label>
          <div class="line">
            <input
              type="range"
              class="w-20"
              bind:value={inches}
              min="53"
              max="76"
            />
            <span class="w-24"
              >{Math.floor(feet)}"{Math.round(feetRemainder)},
              {Math.round(CM)} cm</span
            >
            <input type="text" class="input-text" bind:value={inches} />
          </div>
        </div>
        <div>
          <label class="label">SaO2 (%)</label>
          <div class="line">
            <input type="range" bind:value={SaO2} min="60" max="100" />
            <input type="text" class="input-text" bind:value={SaO2} />
          </div>
        </div>
        <div>
          <label class="label">SvO2 (%)</label>
          <div class="line">
            <input type="range" bind:value={SvO2} min="30" max="100" />
            <input type="text" class="input-text" bind:value={SvO2} />
          </div>
        </div>
        <div>
          <label class="label">Hgb (g/dL)</label>
          <div class="line">
            <input type="range" bind:value={HGB} min="4" max="17" step="0.1" />
            <input type="text" class="input-text" bind:value={HGB} />
          </div>
        </div>
        <div>
          <label class="label">Age (yrs)</label>
          <div class="line">
            <input type="range" bind:value={Age} min="1" max="100" />
            <input type="text" class="input-text" bind:value={Age} />
          </div>
        </div>
        <div>
          <label class="label">MAP (mmHg)</label>
          <div class="line">
            <input type="range" bind:value={MAP} min="50" max="150" />
            <input type="text" class="input-text" bind:value={MAP} />
          </div>
        </div>
        <div>
          <label class="label">CVP (mmHg)</label>
          <div class="line">
            <input type="range" bind:value={CVP} min="1" max="20" />
            <input type="text" class="input-text" bind:value={CVP} />
          </div>
        </div>
        <div />
      </div>
    </div>
  </div>
</main>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  @font-face {
    font-family: "Concourse";
    src: url("fonts/concourse_3_regular.woff2") format("woff2");
    font-style: normal;
  }
  @font-face {
    font-family: "Equity";
    src: url("fonts/equity_a_regular.woff2") format("woff2");
    font-style: normal;
  }
  @layer components {
    body {
      @apply text-zinc-800 text-lg font-serif;
    }
    h1 {
      @apply text-xl font-bold uppercase mb-4;
    }
    h2 {
      @apply text-lg font-bold uppercase mb-2;
    }
    p {
      @apply text-zinc-800 text-lg font-serif;
    }
    ::selection {
      @apply bg-slate-200;
    }
    .btn {
      @apply btn-disabled p-3 shadow font-semibold rounded-lg border-2 border-solid border-zinc-800 bg-zinc-50
		font-sans hover:bg-white text-zinc-800 hover:text-black;
    }
    .btn-dark {
      @apply btn border-0 bg-zinc-800 text-zinc-50 hover:bg-zinc-900 hover:text-white transition duration-300;
    }
    .btn-disabled {
      @apply disabled:text-zinc-500 disabled:hover:text-zinc-500 disabled:bg-zinc-50 disabled:hover:bg-zinc-50 disabled:border-zinc-500 disabled:hover:border-zinc-500;
    }
    .select {
      @apply font-semibold focus:outline-none focus:bg-white focus:border-zinc-800 border-zinc-800 border-2 border-solid bg-zinc-50 text-zinc-800 rounded-md   outline-none focus:ring-0;
    }
    .label {
      @apply font-semibold text-sm;
    }
    .input-text {
      @apply px-2 max-h-12 focus:bg-white focus:ring-0 w-12   focus:border-zinc-800 border-zinc-200  py-2 rounded-md bg-zinc-50  font-semibold;
    }
    .input-checkbox {
      @apply rounded-sm border-2 border-solid w-4 h-4 focus:ring-0  text-zinc-800 border-zinc-800;
    }
    .warning {
      @apply text-red-700;
    }
    input[type="range"] {
      @apply flex-grow;
    }
    .line {
      @apply flex space-x-2 items-center;
    }
  }
</style>
