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


<div style="display: flex; justify-content: space-around; flex-wrap: wrap;">
    <img src="{{ '/assets/Img/search_engine.PNG' | relative_url }}" alt="Image 1" style="width: 50%; margin: 10px;">
</div>
Image was created by Pix-Art-α gen AI model. 
Prompt: "give me a logo of a  Search Engine,  use the colors of a google doodle"
