<!-- App.svelte -->
<script>
  import { onMount } from "svelte";

  // Oscar statuette SVG icon component
  const OscarIcon = ({ className = "" }) => `
    <svg class="${className}" width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M12 3C10.9 3 10 3.9 10 5V8.5C10 9.6 10.9 10.5 12 10.5C13.1 10.5 14 9.6 14 8.5V5C14 3.9 13.1 3 12 3Z" fill="currentColor"/>
      <path d="M12 11.5C9.2 11.5 7 13.7 7 16.5V21H17V16.5C17 13.7 14.8 11.5 12 11.5Z" fill="currentColor"/>
    </svg>
  `;

  // States for the app
  let predictions = {};
  let results = {};
  let score = 0;
  let activeTab = "predict";
  let savedStatus = "";

  // Load data from localStorage on initial render
  onMount(() => {
    const savedPredictions = localStorage.getItem("oscarPredictions2025");
    const savedResults = localStorage.getItem("oscarResults2025");

    if (savedPredictions) {
      predictions = JSON.parse(savedPredictions);
    }

    if (savedResults) {
      results = JSON.parse(savedResults);
      calculateScore(JSON.parse(savedPredictions), JSON.parse(savedResults));
    }
  });

  // Calculate score when results change
  const calculateScore = (preds, res) => {
    let newScore = 0;
    Object.keys(res).forEach((category) => {
      if (preds[category] === res[category]) {
        newScore++;
      }
    });
    score = newScore;
  };

  // Save predictions to localStorage
  const savePredictions = () => {
    localStorage.setItem("oscarPredictions2025", JSON.stringify(predictions));
    savedStatus = "Predictions saved successfully!";
    setTimeout(() => (savedStatus = ""), 3000);
  };

  // Save results to localStorage and calculate score
  const saveResults = () => {
    localStorage.setItem("oscarResults2025", JSON.stringify(results));
    calculateScore(predictions, results);
    savedStatus = "Results saved successfully!";
    setTimeout(() => (savedStatus = ""), 3000);
  };

  // Handle prediction selection
  const handlePredictionChange = (category, nominee) => {
    predictions = {
      ...predictions,
      [category]: nominee,
    };
  };

  // Handle result selection
  const handleResultChange = (category, nominee) => {
    results = {
      ...results,
      [category]: nominee,
    };
  };

  // Reset all data
  const resetAll = () => {
    if (
      window.confirm(
        "Are you sure you want to reset all predictions and results?",
      )
    ) {
      localStorage.removeItem("oscarPredictions2025");
      localStorage.removeItem("oscarResults2025");
      predictions = {};
      results = {};
      score = 0;
      savedStatus = "All data has been reset!";
      setTimeout(() => (savedStatus = ""), 3000);
    }
  };

  // Oscar categories and nominees data from 2025
  const categories = {
    bestPicture: {
      title: "Best Picture",
      nominees: [
        "Anora",
        "The Brutalist",
        "A Complete Unknown",
        "Conclave",
        "Dune: Part Two",
        "Emilia Pérez",
        "I'm Still Here",
        "Nickel Boys",
        "The Substance",
        "Wicked",
      ],
    },
    bestActress: {
      title: "Best Actress",
      nominees: [
        "Cynthia Erivo, Wicked",
        "Karla Sofía Gascón, Emilia Pérez",
        "Mikey Madison, Anora",
        "Demi Moore, The Substance",
        "Fernanda Torres, I'm Still Here",
      ],
    },
    bestDirector: {
      title: "Best Director",
      nominees: [
        "Sean Baker, Anora",
        "Brady Corbet, The Brutalist",
        "James Mangold, A Complete Unknown",
        "Jacques Audiard, Emilia Pérez",
        "Coralie Fargeat, The Substance",
      ],
    },
    bestActor: {
      title: "Best Actor",
      nominees: [
        "Adrien Brody, The Brutalist",
        "Timothée Chalamet, A Complete Unknown",
        "Colman Domingo, Sing Sing",
        "Ralph Fiennes, Conclave",
        "Sebastian Stan, The Apprentice",
      ],
    },
    bestOriginalSong: {
      title: "Best Original Song",
      nominees: [
        '"El Mal" from Emilia Pérez',
        '"Mi Camino" from Emilia Pérez',
        '"Like a Bird" from Sing Sing',
        '"The Journey" from The Six Triple Eight',
        '"Never Too Late" from Elton John: Never Too Late',
      ],
    },
    bestOriginalScore: {
      title: "Best Original Score",
      nominees: [
        "Daniel Blumberg, The Brutalist",
        "Volker Bertelmann, Conclave",
        "Clément Ducol and Camille, Emilia Pérez",
        "John Powell and Stephen Schwartz, Wicked",
        "Kris Bowers, The Wild Robot",
      ],
    },
    bestSound: {
      title: "Best Sound",
      nominees: [
        "A Complete Unknown",
        "Dune: Part Two",
        "Emilia Pérez",
        "Wicked",
        "The Wild Robot",
      ],
    },
    bestLiveActionShort: {
      title: "Best Live-Action Short",
      nominees: [
        '"A Lien"',
        '"Anuja"',
        '"I\'m Not a Robot"',
        '"The Last Ranger"',
        '"The Man Who Could Not Remain Silent"',
      ],
    },
    bestCinematography: {
      title: "Best Cinematography",
      nominees: [
        "The Brutalist",
        "Dune: Part Two",
        "Emilia Pérez",
        "Maria",
        "Nosferatu",
      ],
    },
    bestDocumentaryFeature: {
      title: "Best Documentary Feature",
      nominees: [
        "Black Box Diaries",
        "No Other Land",
        "Porcelain War",
        "Soundtrack to a Coup d'Etat",
        "Sugarcane",
      ],
    },
    bestDocumentaryShort: {
      title: "Best Documentary Short",
      nominees: [
        '"Death by Numbers"',
        '"I Am Ready, Warden"',
        '"Incident"',
        '"Instruments of a Beating Heart"',
        '"The Only Girl in the Orchestra"',
      ],
    },
    bestEditing: {
      title: "Best Editing",
      nominees: [
        "Anora",
        "The Brutalist",
        "Conclave",
        "Emilia Pérez",
        "Wicked",
      ],
    },
    bestVisualEffects: {
      title: "Best Visual Effects",
      nominees: [
        "Alien: Romulus",
        "Better Man",
        "Dune: Part Two",
        "Kingdom of the Planet of the Apes",
        "Wicked",
      ],
    },
    bestSupportingActor: {
      title: "Best Supporting Actor",
      nominees: [
        "Yura Borisov, Anora",
        "Kieran Culkin, A Real Pain",
        "Edward Norton, A Complete Unknown",
        "Guy Pearce, The Brutalist",
        "Jeremy Strong, The Apprentice",
      ],
    },
    bestInternationalFeature: {
      title: "Best International Feature",
      nominees: [
        "Brazil, I'm Still Here",
        "Denmark, The Girl with the Needle",
        "France, Emilia Pérez",
        "Germany, The Seed of the Sacred Fig",
        "Latvia, Flow",
      ],
    },
    bestCostumeDesign: {
      title: "Best Costume Design",
      nominees: [
        "A Complete Unknown",
        "Conclave",
        "Gladiator II",
        "Nosferatu",
        "Wicked",
      ],
    },
    bestProductionDesign: {
      title: "Best Production Design",
      nominees: [
        "The Brutalist",
        "Conclave",
        "Dune: Part Two",
        "Nosferatu",
        "Wicked",
      ],
    },
    bestHairAndMakeup: {
      title: "Best Hair and Makeup",
      nominees: [
        "A Different Man",
        "Emilia Pérez",
        "Nosferatu",
        "The Substance",
        "Wicked",
      ],
    },
    bestAdaptedScreenplay: {
      title: "Best Adapted Screenplay",
      nominees: [
        "James Mangold and Jay Cocks, A Complete Unknown",
        "Peter Straughan, Conclave",
        "Jacques Audiard, in collaboration with Thomas Bidegain, Léa Mysius, and Nicolas Livecchi, Emilia Pérez",
        "RaMell Ross, Joslyn Barnes, Nickel Boys",
        'Clint Bentley and Greg Kwedar, with a story by Clint Bentley, Greg Kwedar, Clarence Maclin, John "Divine G" Whitfield, Sing Sing',
      ],
    },
    bestOriginalScreenplay: {
      title: "Best Original Screenplay",
      nominees: [
        "Sean Baker, Anora",
        "Brady Corbet and Mona Fastvold, The Brutalist",
        "Jesse Eisenberg, A Real Pain",
        "Moritz Binder, Tim Fehlbaum, Alex David, September 5",
        "Coralie Fargeat, The Substance",
      ],
    },
    bestAnimatedFeature: {
      title: "Best Animated Feature",
      nominees: [
        "Flow",
        "Inside Out 2",
        "Memoir of a Snail",
        "Wallace & Gromit: Vengeance Most Fowl",
        "The Wild Robot",
      ],
    },
    bestAnimatedShort: {
      title: "Best Animated Short",
      nominees: [
        '"Beautiful Men"',
        '"In the Shadow of the Cypress"',
        '"Magic Candies"',
        '"Wander to Wonder"',
        '"Yuck!"',
      ],
    },
    bestSupportingActress: {
      title: "Best Supporting Actress",
      nominees: [
        "Monica Barbaro, A Complete Unknown",
        "Ariana Grande, Wicked",
        "Felicity Jones, The Brutalist",
        "Isabella Rossellini, Conclave",
        "Zoe Saldaña, Emilia Pérez",
      ],
    },
  };
</script>

<div class="min-h-screen bg-black">
  <div class="max-w-4xl mx-auto p-4 text-white">
    <header class="mb-8 text-center">
      <div class="flex justify-center mb-2">
        {@html OscarIcon({ className: "text-yellow-500 h-12 w-12 mr-2" })}
        <h1 class="text-4xl font-bold text-yellow-500 font-serif">
          Oscar Predictions 2025
        </h1>
        {@html OscarIcon({ className: "text-yellow-500 h-12 w-12 ml-2" })}
      </div>
      <p class="text-gray-300 mb-6">
        Make your predictions and see how many you get right!
      </p>

      <!-- Decorative element - stars -->
      <div class="flex justify-center mb-6">
        {#each Array(5) as _, i}
          <span key={i} class="text-yellow-500 mx-1">★</span>
        {/each}
      </div>

      <!-- Tabs -->
      <div class="flex justify-center mb-4">
        <button
          on:click={() => (activeTab = "predict")}
          class={`px-4 py-2 mx-1 rounded-md border-2 transition-all duration-300 ${
            activeTab === "predict"
              ? "bg-yellow-500 border-yellow-600 text-black font-bold"
              : "bg-transparent border-yellow-500 text-yellow-500 hover:bg-yellow-500/10"
          }`}
        >
          Make Predictions
        </button>
        <button
          on:click={() => (activeTab = "results")}
          class={`px-4 py-2 mx-1 rounded-md border-2 transition-all duration-300 ${
            activeTab === "results"
              ? "bg-yellow-500 border-yellow-600 text-black font-bold"
              : "bg-transparent border-yellow-500 text-yellow-500 hover:bg-yellow-500/10"
          }`}
        >
          Enter Results
        </button>
      </div>

      <!-- Score display -->
      {#if Object.keys(results).length > 0}
        <div
          class="mb-4 p-3 bg-yellow-500/20 border border-yellow-500 rounded-md inline-block"
        >
          <p class="font-bold text-yellow-500">
            Your Score: {score} / {Object.keys(categories).length} correct ({Math.round(
              (score / Object.keys(categories).length) * 100,
            )}%)
          </p>
        </div>
      {/if}

      <!-- Save status message -->
      {#if savedStatus}
        <div
          class="mb-4 p-2 bg-yellow-500/20 text-yellow-500 border border-yellow-500 rounded-md"
        >
          {savedStatus}
        </div>
      {/if}
    </header>

    <main>
      <!-- Instructions -->
      {#if activeTab === "predict"}
        <div
          class="mb-6 p-4 bg-gray-900 border border-yellow-500/50 rounded-lg"
        >
          <h2 class="text-xl font-semibold mb-2 text-yellow-500">
            Make Your Predictions
          </h2>
          <p class="text-gray-300">
            Select who you think will win in each category. Your predictions
            will be saved in your browser.
          </p>
          <button
            on:click={savePredictions}
            class="mt-3 px-4 py-2 bg-yellow-500 text-black font-bold rounded-md hover:bg-yellow-400 transition-colors"
          >
            Save Predictions
          </button>
        </div>
      {:else}
        <div
          class="mb-6 p-4 bg-gray-900 border border-yellow-500/50 rounded-lg"
        >
          <h2 class="text-xl font-semibold mb-2 text-yellow-500">
            Enter the Winners
          </h2>
          <p class="text-gray-300">
            After the Oscars, enter the actual winners to see how well you did.
          </p>
          <button
            on:click={saveResults}
            class="mt-3 px-4 py-2 bg-yellow-500 text-black font-bold rounded-md hover:bg-yellow-400 transition-colors"
          >
            Save Results
          </button>
        </div>
      {/if}

      <!-- Categories -->
      {#each Object.entries(categories) as [categoryKey, categoryData]}
        <div
          class="mb-6 p-4 bg-gray-900 border border-gray-800 rounded-lg shadow-lg"
        >
          <div class="flex items-center mb-3">
            {@html OscarIcon({ className: "text-yellow-500 h-5 w-5 mr-2" })}
            <h3 class="text-xl font-medium text-yellow-500">
              {categoryData.title}
            </h3>
          </div>
          <div class="space-y-3">
            {#each categoryData.nominees as nominee, idx}
              <div
                class="flex items-center p-2 hover:bg-gray-800 rounded-md transition-colors"
              >
                <input
                  type="radio"
                  id={`${activeTab}-${categoryKey}-${idx}`}
                  name={`${activeTab}-${categoryKey}`}
                  value={nominee}
                  checked={activeTab === "predict"
                    ? predictions[categoryKey] === nominee
                    : results[categoryKey] === nominee}
                  on:change={() => {
                    if (activeTab === "predict") {
                      handlePredictionChange(categoryKey, nominee);
                    } else {
                      handleResultChange(categoryKey, nominee);
                    }
                  }}
                  class="mr-3 accent-yellow-500 h-4 w-4"
                />
                <label
                  for={`${activeTab}-${categoryKey}-${idx}`}
                  class={`flex-1 cursor-pointer ${
                    activeTab === "results" &&
                    predictions[categoryKey] === nominee &&
                    results[categoryKey] === nominee
                      ? "font-bold text-yellow-500"
                      : "text-gray-200"
                  }`}
                >
                  {nominee}
                  {#if activeTab === "results" && predictions[categoryKey] === nominee && results[categoryKey] === nominee}
                    <span> ★ (correct)</span>
                  {/if}
                </label>
              </div>
            {/each}
          </div>
        </div>
      {/each}
    </main>

    <footer class="mt-12 pt-6 border-t border-gray-800 text-center">
      <button
        on:click={resetAll}
        class="px-4 py-2 bg-red-600 text-white rounded-md hover:bg-red-700 transition-colors"
      >
        Reset All Data
      </button>

      <!-- Decorative element - stars -->
      <div class="flex justify-center my-6">
        {#each Array(5) as _, i}
          <span key={i} class="text-yellow-500 mx-1">★</span>
        {/each}
      </div>

      <p class="mt-2 text-gray-400 text-sm">
        Oscar predictions data is stored locally in your browser.
        <br />
        No data is sent to any server.
      </p>

      <p class="mt-6 text-yellow-500 text-sm font-serif">
        97th Academy Awards • March 2, 2025
      </p>
    </footer>
  </div>
</div>
