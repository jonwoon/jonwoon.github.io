---
layout: page
permalink: /publications/
title: publications
description: 
nav: true
nav_order: 3
---
<!-- _pages/publications.md -->
<style>
/* Modify the default post-header to have the filter on the right - scoped to publications page */
body.publications-page .post-header,
.publications-page-wrapper .post-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}
body.publications-page .post-header h1,
.publications-page-wrapper .post-header h1 {
  margin-bottom: 0;
  flex: 1;
}
.publications-filter {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
.publications-filter label {
  margin-bottom: 0;
  white-space: nowrap;
}
.publications-filter select {
  min-width: 200px;
}
@media (max-width: 768px) {
  body.publications-page .post-header,
  .publications-page-wrapper .post-header {
    flex-direction: column;
    align-items: flex-start;
  }
  .publications-filter {
    width: 100%;
    margin-top: 0.5rem;
  }
  .publications-filter select {
    width: 100%;
  }
}
</style>

<div class="publications publications-page">
  <div id="publications-list">
    {% bibliography %}
  </div>
</div>

<script>
// Add filter to the default post-header and set up filter functionality
(function() {
  function initFilter() {
    // Check if we're on the publications page
    const publicationsPage = document.querySelector('.publications-page');
    if (!publicationsPage) return;
    
    // Add class to body for CSS scoping
    document.body.classList.add('publications-page');
    
    // Find the post-header (it's a sibling of the article, not a child)
    const postHeader = document.querySelector('.post-header');
    if (!postHeader) return;
    
    // Check if filter already exists
    if (document.getElementById('category-filter')) return;
    
    const filterHtml = `
      <div class="publications-filter">
        <label for="category-filter" class="form-label mb-0">Filter:</label>
        <select id="category-filter" class="form-control">
          <option value="all">All Publications</option>
          <option value="Experimental Political Economy">Experimental Political Economy</option>
          <option value="Beliefs and Communication">Beliefs and Communication</option>
          <option value="Voting and Elections">Voting and Elections</option>
          <option value="Parties and Partisanship">Parties and Partisanship</option>
          <option value="Legislative Politics">Legislative Politics</option>
          <option value="Methodological">Methodological</option>
        </select>
      </div>
    `;
    postHeader.insertAdjacentHTML('beforeend', filterHtml);
    
    // Set up filter functionality after adding the filter
    const filter = document.getElementById('category-filter');
    const publicationsList = document.getElementById('publications-list');
    
    if (!filter || !publicationsList) return;
  
    function filterPublications() {
      const selectedCategory = filter.value;
      const entries = publicationsList.querySelectorAll('.row[data-categories]');
      const yearHeaders = publicationsList.querySelectorAll('h2.bibliography');
      
      // Filter entries
      entries.forEach(function(entry) {
        if (selectedCategory === 'all') {
          entry.style.display = '';
        } else {
          const categories = entry.getAttribute('data-categories') || '';
          const categoryArray = categories.split(',').map(c => c.trim());
          
          if (categoryArray.includes(selectedCategory)) {
            entry.style.display = '';
          } else {
            entry.style.display = 'none';
          }
        }
      });
      
      // Hide year headers that have no visible publications
      yearHeaders.forEach(function(header) {
        let hasVisiblePublications = false;
        let currentElement = header.nextElementSibling;
        
        // Check all siblings until we hit the next year header or end
        while (currentElement) {
          if (currentElement.classList && currentElement.classList.contains('bibliography')) {
            // Hit the next year header, stop checking
            break;
          }
          if (currentElement.classList && currentElement.classList.contains('row')) {
            // Check if this is a visible publication entry
            if (currentElement.style.display !== 'none' && 
                currentElement.hasAttribute('data-categories')) {
              hasVisiblePublications = true;
              break;
            }
          }
          currentElement = currentElement.nextElementSibling;
        }
        
        // Hide header if no visible publications
        if (selectedCategory === 'all') {
          header.style.display = '';
        } else {
          header.style.display = hasVisiblePublications ? '' : 'none';
        }
      });
    }
    
    filter.addEventListener('change', filterPublications);
  }
  
  // Try immediately and also on DOMContentLoaded
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initFilter);
  } else {
    initFilter();
  }
})();
</script>
