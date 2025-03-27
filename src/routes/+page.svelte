<script>
    // Definir tu token de GitHub aquí
    // githubToken = "cambiar a variable de entorno :v";
    
    // Función para hacer el fetch con autenticación
//     async function fetchGithubData(username) {
//       const response = await fetch(`https://api.github.com/users/${username}`, {
//         headers: {
//           'Authorization': `token ${githubToken}`
//         }
//       });
//       return response;
//     }
//   </script>
  
  <h1>Home</h1>
  

  <h1> Andres De La Puente Ancco</h1>
   
  <img src="./images/FOTO.jpeg" alt="mike" width="500px">
   
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
          <!-- Puedes agregar más estadísticas que te interesen -->
        </dl>
      </section>
    {:catch error}
      <p class="error">Something went wrong: {error.message}</p>
    {/await}
  {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
  {/await}