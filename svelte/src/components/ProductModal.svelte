<script>
    import { onMount } from 'svelte';
  
    let products = [];
  
    onMount(async () => {
      try {
        const response = await fetch('https://fakestoreapi.com/products');
        if (response.ok) {
          products = await response.json();
        } else {
          console.error('Failed to retrieve product data');
        }
      } catch (error) {
        console.error('Error fetching product data:', error);
      }
    });
  </script>
  
  <h1>Products from Fake Store API</h1>
  
  {#if products.length > 0}
    <ul>
      {#each products as product}
        <li>{product.title} - ${product.price} - {product.category}</li>
      {/each}
    </ul>
  {:else}
    <p>Loading products...</p>
  {/if}
  

