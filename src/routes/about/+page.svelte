<script>
  import "../..//app.css";
</script>

<div class="wrapper">
  <div class="content flex flex-col m-3">
    <div>
      <h1 class="">
        This website is made of the Berkeley F24 Phil25a class final exam study
      </h1>
      <h1 class="text-center">
        Find out more about me <a href="stephenokita.com">here</a>
      </h1>
      <h1 class="text-center">
        Contribute to this <a
          href="https://github.com/TheArctesian/25A-Final.git">here</a
        >
      </h1>
    </div>
    <div class="back">
      <a href="/" class="colors">Back to app</a>
    </div>
  </div>
</div>

<style>
  .back {
    margin: auto;
    border-radius: 0.2rem;
    font-weight: bold;
    padding: 0.5rem;
    width: fit-content;
    transition: all ease-in-out 100ms;
  }
  .back:hover {
    color: var(--purple) !important;
    transition: all ease-in-out 100ms;
  }
  .donate {
    width: fit-content;
    padding: 1rem;
    background-color: var(--purple);
    color: white;
    inline-size: 80vw;
    overflow: auto;
    border-radius: 0.2rem;
  }
  a {
    color: var(--comment);
    transition: all ease-in-out 200ms;
  }
  .link {
    font-style: italic;
  }
  .link:hover {
    transition: all ease-in-out 200ms;
    font-style: normal;
  }

  .projects {
    margin: auto;
    text-decoration: none;
    font-size: 5vw;
    transition: all ease-in-out 400ms;
    color: var(--purple);
  }

  :global(body) {
    background-color: var(--bg);
    font-family: "Fira Code", monospace;
    letter-spacing: 0.1em; /* Adjust as needed */
  }
  .wrapper {
    display: flex;
    height: 100vh;
    justify-content: center;
    align-items: center;
  }
  .content {
    justify-content: center;
    flex-direction: column;
    align-items: center;
    font-size: small;
    display: flex;
    padding: 1rem;
    margin: 1rem;
    background-color: var(--fg);
  }
  .element {
    margin: 1rem;
    border-radius: 0.2rem;
    padding: 1rem;
    border: 0.2rem solid var(--fg);
  }
  .element:hover {
    border: 0.2rem solid var(--purple);
  }

  @media (max-width: 750px) {
    .element {
      margin: 0.6rem;
      border-radius: 0rem;
      padding: 0rem;
      border: 0.2rem solid var(--fg);
    }
  }
  .text {
    background-color: var(--fg);
    padding: 1rem;
    border-radius: 1rem;
  }
  img {
    transition: all ease-in-out 200ms;
    width: 7.5vw;
  }
  .s {
    padding: 1rem;
    margin: auto;
    width: 30vw;
  }
  .colors {
    color: var(--selection);
  }
</style>
