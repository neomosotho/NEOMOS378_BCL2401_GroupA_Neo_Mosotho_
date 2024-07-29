<script>
  import { onMount } from 'svelte';
  import NavBar from './NavBar.svelte';
  import ProductGrid from './ProductGrid.svelte';
  import ProductModal from './ProductModal.svelte';

  let open = false;
  let modalOpen = false;
  let products = [];
  let filteredProducts = [];
  let singleProduct = {};
  let categories = [];
  let selectedCategory = '';
  let sortOrder = 'asc';
  let searchQuery = '';
  let loading = true;

  onMount(() => {
      fetchCategories();
      fetchProducts();
  });

  async function fetchCategories() {
      try {
          const response = await fetch('https://fakestoreapi.com/products/categories');
          categories = await response.json();
      } catch (error) {
          console.error('Error fetching categories:', error);
      }
  }

  async function fetchProducts() {
      loading = true;
      let url = 'https://fakestoreapi.com/products';
      if (selectedCategory) {
          url += `/category/${selectedCategory}`;
      }
      try {
          const response = await fetch(url);
          products = await response.json();
          filterProducts();
          sortProducts();
      } catch (error) {
          console.error('Error fetching products:', error);
      } finally {
          loading = false;
      }
  }

  async function fetchSingleProduct(productId) {
      loading = true;
      try {
          const response = await fetch(`https://fakestoreapi.com/products/${productId}`);
          singleProduct = await response.json();
          modalOpen = true;
      } catch (error) {
          console.error('Error fetching single product:', error);
      } finally {
          loading = false;
      }
  }

  function sortProducts() {
      filteredProducts.sort((a, b) => sortOrder === 'asc' ? a.price - b.price : b.price - a.price);
  }

  function searchProducts() {
      filterProducts();
      sortProducts();
  }

  function filterProducts() {
      filteredProducts = products.filter(product => 
          product.title.toLowerCase().includes(searchQuery.toLowerCase()));
  }
</script>

<NavBar {open} />
<div class="container mx-auto p-4">
  <div class="flex justify-between items-center mb-4">
      <div class="flex space-x-4">
          <select bind:value={selectedCategory} on:change={fetchProducts}>
              <option value="">All Categories</option>
              {#each categories as category}
                  <option value={category}>{category}</option>
              {/each}
          </select>
          <select bind:value={sortOrder} on:change={sortProducts}>
              <option value="asc">Price: Low to High</option>
              <option value="desc">Price: High to Low</option>
          </select>
          <input type="text" bind:value={searchQuery} placeholder="Search products" on:input={searchProducts} class="border rounded px-2 py-1" />
      </div>
  </div>
  {#if loading}
      <p>Loading...</p>
  {:else}
      <ProductGrid {filteredProducts} {fetchSingleProduct} />
  {/if}
</div>
<ProductModal {singleProduct} bind:modalOpen={modalOpen} />

