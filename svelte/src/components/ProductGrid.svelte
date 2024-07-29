// Filter products
    function filterProducts() {
        filteredProducts = products.filter(product => 
            product.title.toLowerCase().includes(searchQuery.toLowerCase()));
    }