<script>
  import Typewriter from "svelte-typewriter";
  import questionBankData from "./questionbank.json";

  /**
   * @type {{ [key: string]: { [key: string]: string[] } }}
   */
  const questionBank = questionBankData;

  let question = "Select a philosopher to begin";
  let philosopher = "";
  let book = "";
  let askedQuestions = new Set();
  let hasStarted = false;

  // Filter states
  let selectedPhilosopher = "";
  let selectedBook = "";

  // Get list of philosophers and their books
  const philosophers = ["All", ...Object.keys(questionBank)];
  $: availableBooks =
    selectedPhilosopher && selectedPhilosopher !== "All"
      ? ["All", ...Object.keys(questionBank[selectedPhilosopher])]
      : [];

  function getAllQuestions() {
    /**
     * @type {{ philosopher: string; book: string; question: any; }[]}
     */
    let allQuestions = [];

    if (selectedPhilosopher === "All") {
      // Get all questions from all philosophers
      for (let phil in questionBank) {
        for (let b in questionBank[phil]) {
          questionBank[phil][b].forEach((q) => {
            allQuestions.push({
              philosopher: phil,
              book: b,
              question: q,
            });
          });
        }
      }
      return allQuestions;
    }

    if (!selectedPhilosopher) return allQuestions;

    if (selectedBook === "All" || !selectedBook) {
      // Get all questions from selected philosopher
      for (let book in questionBank[selectedPhilosopher]) {
        questionBank[selectedPhilosopher][book].forEach((q) => {
          allQuestions.push({
            philosopher: selectedPhilosopher,
            book: book,
            question: q,
          });
        });
      }
    } else {
      // Get questions from specific book
      questionBank[selectedPhilosopher][selectedBook].forEach((q) => {
        allQuestions.push({
          philosopher: selectedPhilosopher,
          book: selectedBook,
          question: q,
        });
      });
    }
    return allQuestions;
  }

  function startQuestions() {
    if (!selectedPhilosopher) {
      alert("Please select a philosopher first");
      return;
    }
    hasStarted = true;
    genRandomQuestion();
  }

  function genRandomQuestion() {
    const allQuestions = getAllQuestions();
    const availableQuestions = allQuestions.filter(
      (q) => !askedQuestions.has(q.question)
    );

    if (availableQuestions.length === 0) {
      question =
        "No more questions available for this selection. Please refresh to start over.";
      philosopher = "";
      return;
    }

    const randomIndex = Math.floor(Math.random() * availableQuestions.length);
    const selectedQuestion = availableQuestions[randomIndex];

    askedQuestions.add(selectedQuestion.question);
    philosopher = selectedQuestion.philosopher;
    book = selectedQuestion.book;
    question = selectedQuestion.question;
  }

  function goBackToFilters() {
    hasStarted = false;
    selectedPhilosopher = "";
    selectedBook = "";
    askedQuestions.clear();
    question = "Select a philosopher to begin";
    philosopher = "";
    book = "";
  }
</script>

<div class="card margin-auto text-center flex-col">
  <div class="c flex flex-col justifty-between">
    <div class="cc">
      <h1 class="text-center m-auto text-2xl">
        <b class="text-3xl">Question: </b><br />
      </h1>
    </div>

    {#if !hasStarted}
      <div class="selection-screen">
        <div class="select-group">
          <label for="philosopher">Philosopher:</label>
          <select
            id="philosopher"
            bind:value={selectedPhilosopher}
            class="select-style"
          >
            <option value="">Choose a philosopher</option>
            {#each philosophers as phil}
              <option value={phil}>{phil}</option>
            {/each}
          </select>
        </div>

        {#if selectedPhilosopher && selectedPhilosopher !== "All"}
          <div class="select-group">
            <label for="book">Text:</label>
            <select id="book" bind:value={selectedBook} class="select-style">
              <option value="">Choose a text</option>
              {#each availableBooks as book}
                <option value={book}>{book}</option>
              {/each}
            </select>
          </div>
        {/if}

        <button class="but mt-4" on:click={startQuestions}>
          Start Questions
        </button>
      </div>
    {:else}
      <div class="cc">
        <!-- {#if philosopher}
          <div class="font-bold">
            <Typewriter interval={10}>
              <h1>{philosopher}</h1>
            </Typewriter>
          </div>
          <div class="text-sm text-gray-500">
            <Typewriter interval={10}>
              <p>From: {book}</p>
            </Typewriter>
          </div>
        {/if}
-->
        <Typewriter interval={10}> 
          <h1 class="bold">{question}</h1>
        </Typewriter>
      </div>

      <div class="button-group">
        <button class="but" on:click={genRandomQuestion}>
          Get a new question
        </button>
        <button class="butt" on:click={goBackToFilters}>
          Back to Filters
        </button>
      </div>
    {/if}

    <div class="cc">
      <a href="/about" class="text-l underline a">About this website</a>
    </div>
  </div>
</div>

<style>
  .a {
    color: var(--selection);
  }
  .cc {
    margin: 1rem;
  }
  .c {
    margin: auto;
    justify-content: space-between;
    background-color: var(--fg);
    padding: 1rem;
    border-radius: 0.2rem;
  }
  .but {
    margin: auto;
    padding: 1rem;
    border-radius: 0.2rem;
    color: white;
    background-color: var(--bg);
    transition: all ease-in-out 200ms;
  }
  .but:hover {
    transition: all ease-in-out 200ms;
  }
  .butt {
    margin: auto;
    padding: 1rem;
    border-radius: 0.2rem;
    transition: all ease-in-out 200ms;
  }
  .butt:hover {
    color: white;
    background-color: var(--bg);
    transition: all ease-in-out 200ms;
  }
  .card {
    display: flex;
    height: 96vh;
    margin: 1rem;
    padding: 1rem;
    border-radius: 1rem;
  }

  .selection-screen {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    padding: 2rem;
  }

  .select-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    margin-bottom: 1rem;
  }

  .select-style {
    padding: 0.5rem;
    border-radius: 0.2rem;
    border: 1px solid var(--selection);
    background-color: var(--fg);
    color: inherit;
    width: 100%;
    max-width: 300px;
    margin: 0 auto;
  }

  label {
    text-align: center;
    color: inherit;
  }

  .mt-4 {
    margin-top: 1rem;
  }

  .button-group {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: center;
    margin: 1rem;
  }

  .secondary {
    background-color: transparent;
    border: 1px solid var(--bg);
  }

  .secondary:hover {
    background-color: var(--bg);
  }
</style>
