<script>
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
  
{#await fetchGithubData("andresdlp05")}
  <p>Loading...</p>
{:then response}
  {#await response.json()}
    <p>Decoding...</p>
  {:then data}
    <section>
      <h2>My GitHub Stats</h2>
      <dl>
        <dt>Followers</dt>
        <dd>{data.followers}</dd>
        <dt>Following</dt>
        <dd>{data.following}</dd>
        <dt>Public Repos</dt>
        <dd>{data.public_repos}</dd>
        <!-- Puedes añadir más estadísticas si quieres -->
        <dt>Location</dt>
        <dd>{data.location || 'Not specified'}</dd>
        <dt>Account Created</dt>
        <dd>{new Date(data.created_at).toLocaleDateString()}</dd>
      </dl>
    </section>
  {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
  {/await}
{:catch error}
  <p class="error">Something went wrong: {error.message}</p>
{/await}

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