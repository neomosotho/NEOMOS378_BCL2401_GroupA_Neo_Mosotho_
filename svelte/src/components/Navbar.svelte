<!-- NavBar.svelte -->
<script>
    let open = false;
  </script>
  
  <nav class="border border-slate-600 shadow-md p-4 flex justify-between items-center">
      <div class="text-2xl font-extrabold text-slate-900">
          SwiftCraft
      </div>
      <!-- Desktop Menu -->
      <div class="hidden lg:flex space-x-4">
          <a href="#" class="text-lg font-semibold">WishList</a>
          <i class="fas fa-shopping-cart" aria-hidden="true"></i>
          <a href="#" class="text-lg font-semibold">LogIn</a>
      </div>
      <!-- Toggle Bar -->
      <div>
          <button on:click={() => open = !open} class="lg:hidden text-black focus:outline-none bg-slate-700 p-2 rounded" aria-label="Toggle menu">
              <i class="fas fa-bars text-2xl" aria-hidden="true"></i>
          </button>
      </div>
      <!-- Mobile Menu -->
      {#if open}
        <div class="lg:hidden">
            <a href="#" class="block text-lg font-semibold py-2">WishList</a>
            <i class="fas fa-shopping-cart block py-2" aria-hidden="true"></i>
            <a href="#" class="block text-lg font-semibold py-2">LogIn</a>
        </div>
      {/if}
  </nav>
  
  <style>
    /* Add any component-specific styles here */
  </style>
  