<script>
  import Typewriter from "svelte-typewriter";
  import questionBankData from "./questionbank.json";

  /**
   * @type {{ [key: string]: { [key: string]: string[] } }}
   */
  const questionBank = questionBankData;

  let question = "Press the button to get a question";
  let philosopher = "";
  let book = "";
  let askedQuestions = new Set();

  function getAllQuestions() {
    /**
     * @type {{ philosopher: string; book: string; question: any; }[]}
     */
    let allQuestions = [];

    // Iterate through philosophers
    for (let philosopher in questionBank) {
      // Iterate through books
      for (let book in questionBank[philosopher]) {
        // Add all questions from this book
        questionBank[philosopher][book].forEach((q) => {
          allQuestions.push({
            philosopher: philosopher,
            book: book,
            question: q,
          });
        });
      }
    }
    return allQuestions;
  }

  function genRandomQuestion() {
    const allQuestions = getAllQuestions();
    const availableQuestions = allQuestions.filter(
      (q) => !askedQuestions.has(q.question)
    );

    if (availableQuestions.length === 0) {
      question =
        "You have exhausted all the questions. Refresh the tab to start over.";
      return;
    }

    const randomIndex = Math.floor(Math.random() * availableQuestions.length);
    const selectedQuestion = availableQuestions[randomIndex];

    askedQuestions.add(selectedQuestion.question);
    philosopher = selectedQuestion.philosopher;
    question = selectedQuestion.question;
  }
</script>

<div class="card margin-auto text-center flex-col">
  <div class="c flex flex-col justifty-between">
    <div class="cc">
      <h1 class="text-center m-auto text-2xl">
        <b class="text-3xl">Question: </b><br />
      </h1>
    </div>
    <div class="cc">
      {#if philosopher}
        <div class="font-bold">
          <Typewriter interval={10}>
            <h1 >{philosopher}</h1>
          </Typewriter>
        </div>
      {/if}

      <Typewriter interval={10}>
        <h1 class="bold">{question}</h1>
      </Typewriter>
    </div>
    <div class="cc">
      <button class="but" on:click={genRandomQuestion}
        >Get a new question</button
      >
    </div>
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
  .card {
    display: flex;
    height: 96vh;
    margin: 1rem;
    padding: 1rem;
    border-radius: 1rem;
  }
</style>
