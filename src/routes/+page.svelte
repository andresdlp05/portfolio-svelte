<script>
import { onMount } from "svelte";

let githubData = null;
let loading = true;
let error = null;

onMount(async () => {
    try {
        const response = await fetch("https://api.github.com/users/andresdlp05");
        githubData = await response.json();
    } catch (err) {
        error = err;
    }
    loading = false;
});




// Acceder a la variable de entorno
const githubToken = import.meta.env.VITE_GITHUB_TOKEN;
    
// Función para hacer fetch con la API de GitHub
async function fetchGithubData(username) {
  // Verifica si hay un token disponible
  const headers = githubToken 
    ? { 'Authorization': `token ${githubToken}` }
    : {};
        
  const response = await fetch(`https://api.github.com/users/${username}`, { headers });
  return response;
}
  </script>
  
  <h1>Home</h1>

  <h1>Andres De La Puente Ancco</h1>
   
  <img src="./images/FOTO.jpeg" alt="Andres" width="500px">
   
  <p>Hi, I'm Andrés, a Systems Engineer from the National University of Engineering. I am a specialist in Data Science, Machine Learning Engineering, Data Engineering, DevOps and MLOps with more than 7 years of experience. I have taken a diploma and specializations on the world of data.
   
   For contact by email you can write to me at vdelapuentea@gmail.com / https://wa.me/+51904586708
  </p>
  
{#if loading}
  <p>Loading...</p>
{:else if error}
  <p class="error">Something went wrong: {error.message}</p>
{:else}
  <section>
      <h2>My GitHub Stats</h2>
      <dl>
          <dt>Followers</dt>
          <dd>{githubData.followers}</dd>
          <dt>Following</dt>
          <dd>{githubData.following}</dd>
          <dt>Public Repositories</dt>
          <dd>{githubData.public_repos}</dd>
      </dl>
  </section>
{/if}


<style>
  dl {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
  }
  
  dt {
    grid-row: 1;
    font-family: inherit;
    font-weight: bold;
    color: var(--border-gray);
    text-transform: uppercase;
  }
  
  dd {
    grid-row: 2;
    font-family: inherit;
    font-weight: bold;
    text-align: center;
  }
  
  section {
    border-width: 0.15em;
    border-style: solid;
    border-color: var(--border-gray);
    padding: 1em;
    margin-top: 2em;
    margin-bottom: 2em;
    border-radius: 8px;
  }
  
  .error {
    color: red;
    font-weight: bold;
  }
  
  h2 {
    text-align: center;
    margin-bottom: 1em;
  }
</style>