---
layout: default
title: Search
---

<h1>Search the site</h1>
<input type="text" id="search-input" placeholder="Type to search..." />
<ul id="results-container"></ul>

<script src="https://cdnjs.cloudflare.com/ajax/libs/simple-jekyll-search/1.7.2/simple-jekyll-search.min.js"></script>
<script>
    SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        json: '{{ "/search.json" | absolute_url }}',
        searchResultTemplate: '<li><a href="{url}">{title}</a></li>',
        noResultsText: 'No results found'
    });
</script>










***
***
