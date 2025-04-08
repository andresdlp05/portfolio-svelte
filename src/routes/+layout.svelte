<script>
	import { base } from "$app/paths";
	import { page } from "$app/stores";
	
	// Seguridad para localStorage
	let localStorage = globalThis.localStorage ?? {};
	
	// Esquema de color con fallback desde localStorage
	let colorScheme = localStorage.colorScheme ?? "light dark";
	
	// Referencia al elemento HTML raíz
	let root = globalThis?.document?.documentElement;
	
	// Declaraciones reactivas
	$: root?.style.setProperty("color-scheme", colorScheme);
	$: localStorage.colorScheme = colorScheme;
	
	// Array de páginas para la navegación
	let pages = [
	  { url: "/", title: "Home" },
	  { url: "/projects", title: "Projects" },
	  { url: "/contact", title: "Contact" },
	  { url: "/resume", title: "Resume" },
	  { url: "/meta", title: "Meta" },
	  { url: "https://github.com/andresdlp05", title: "Github" }
	];
  </script>
  
  <nav>
	{#each pages as p}
	  <a 
		href={p.url.startsWith("http") ? p.url : `${base}${p.url}`}
		class:current={$page.route.id === p.url}
		target={p.url.startsWith("http") ? "_blank" : undefined}
	  >
		{p.title}
	  </a>
	{/each}
	
	<label class="color-scheme">
	  Theme:
	  <select bind:value={colorScheme}>
		<option value="light dark">Automatic</option>
		<option value="light">Light</option>
		<option value="dark">Dark</option>
	  </select>
	</label>
  </nav>
  
  <slot />
  
  <style>
	nav {
	  display: flex;
	  justify-content: space-between;
	  padding: 1rem;
	}
	
	nav a {
	  margin-right: 1rem;
	  text-decoration: none;
	  color: var(--text-color);
	}
	
	.current {
	  font-weight: bold;
	  text-decoration: underline;
	}
	
	.color-scheme {
	  margin-left: auto;
	  display: flex;
	  align-items: center;
	}
	
	.color-scheme select {
	  margin-left: 0.5rem;
	  border: none;
	  background-color: var(--background-color);
	  color: var(--text-color);
	  font-size: 1rem;
	  padding: 0.25rem;
	}
  </style>