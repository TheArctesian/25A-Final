<script>
  import "../app.css";
  import { dev } from "$app/environment";
  import { injectAnalytics } from "@vercel/analytics/sveltekit";

  injectAnalytics({ mode: dev ? "development" : "production" });
</script>

<slot />

<style>
  :global(body) {
    background-color: var(--bg);
    font-family: "Fira Code", monospace;
    letter-spacing: 0.1em; /* Adjust as needed */
  }
</style>
