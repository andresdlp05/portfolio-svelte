<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    
    // Definimos los datos
    let data = [
      { value: 1, label: "apples" },
      { value: 2, label: "oranges" },
      { value: 3, label: "mangos" },
      { value: 4, label: "pears" },
      { value: 5, label: "limes" },
      { value: 5, label: "cherries" }
    ];
    
    // Configuramos los generadores
    let colors = d3.scaleOrdinal(d3.schemeTableau10);
    let arcGenerator = d3.arc().innerRadius(0).outerRadius(50);
    let sliceGenerator = d3.pie().value(d => d.value);
    
    // Generamos los datos de los arcos
    let arcData = sliceGenerator(data);
    
    // Estado para el elemento seleccionado
    let selectedIndex = -1;
    
    // Manejar la selección de un elemento
    function handleSelect(index) {
      if (selectedIndex === index) {
        // Si ya está seleccionado, deseleccionamos
        selectedIndex = -1;
      } else {
        // Si no está seleccionado, lo seleccionamos
        selectedIndex = index;
      }
    }
    
    // Determinar si un elemento está seleccionado
    function isSelected(index) {
      return selectedIndex === index;
    }
  </script>
  
  <div class="container">
    <svg viewBox="-50 -50 100 100">
      {#each arcData as slice, index}
        <path 
          d={arcGenerator(slice)} 
          fill={colors(index)}
          class:selected={isSelected(index)}
          on:click={() => handleSelect(index)}
        />
      {/each}
    </svg>
    
    <ul class="legend">
      {#each data as d, index}
        <li 
          class:selected={isSelected(index)}
          on:click={() => handleSelect(index)}
        >
          <span class="swatch" style="--color: {colors(index)}"></span>
          {d.label} <em>({d.value})</em>
        </li>
      {/each}
    </ul>
  </div>
  
  <style>
    /* Contenedor principal que organiza el gráfico y la leyenda en horizontal */
    .container {
      display: flex;
      align-items: center;
      gap: 0.6em;
      flex-wrap: wrap; /* Para dispositivos pequeños */
    }
    
    /* Estilos para el SVG */
    svg {
      max-width: 20em;
      margin-block: 2em;
      /* No recortar formas fuera del viewBox */
      overflow: visible;
    }
    
    /* Reducir la opacidad de paths no seleccionados al hacer hover */
    svg:has(path:hover) path:not(:hover) {
      opacity: 50%;
    }
    
    /* Transición suave para los paths */
    path {
      transition: 300ms;
      cursor: pointer;
    }
    
    /* Estilos para la leyenda */
    .legend {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(8em, 1fr));
      border: 1px solid black;
      padding: 1em;
      margin: 1em;
      flex: 1; /* Ocupar todo el espacio disponible */
      list-style: none;
    }
    
    /* Estilos para los elementos de la leyenda */
    .legend li {
      display: flex;
      align-items: center;
      gap: 0.6em;
      cursor: pointer;
    }
    
    /* Estilos para los swatches */
    .swatch {
      display: inline-block;
      background-color: var(--color);
      width: 1em;
      height: 1em;
      border-radius: 3px;
    }
    
    /* Cuando un path está seleccionado, reducir opacidad de los no seleccionados */
    svg:has(.selected) path:not(.selected) {
      opacity: 50%;
    }
    
    /* Estilos para elementos seleccionados */
    .selected {
      --selected-color: oklch(60% 45% 0);
    }
    
    /* Estilo para paths seleccionados */
    path.selected {
      fill: var(--selected-color) !important;
    }
    
    /* Estilo para elementos de leyenda seleccionados */
    li.selected {
      color: var(--selected-color);
    }
    
    /* Atenuar elementos de leyenda no seleccionados */
    ul:has(.selected) li:not(.selected) {
      color: gray;
    }
    
    /* Asegurar opacidad completa para elementos con hover */
    path:hover {
      opacity: 100% !important;
    }
    
    /* Responsividad para pantallas pequeñas */
    @media (max-width: 600px) {
      .container {
        flex-direction: column;
      }
      
      svg {
        max-width: 100%;
      }
      
      .legend {
        width: 100%;
        margin: 0.5em 0;
      }
    }
  </style>