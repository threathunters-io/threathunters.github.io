---
title: Tools
menu: {main: {weight: 30}}
---

<div class="container push-down mb-5">
  <div class="row mb-2">
    <div class="col-12">
      <h1 class="font-weight-bold">Tools</h1>
    </div>
  </div>
  <div class="row">
    <div class="col">
      <div class="form-group">
        <h5><label class="d-block" for="search">Search</label></h5>
        <input type="search" placeholder="Search query" class="form-control" 
          id="search" oninput="updateExampleItems()">
      </div>
    </div>
    <div class="col">
      <div class="form-group">
        <h5><label class="d-block" for="filter-lang">Filter by operating system</label></h5>
        <select class="form-control" id="filter-lang" onchange="updateExampleItems()">
          <option value="" selected disabled>Loading...</option>
        </select>
      </div>
    </div>
  </div>

  <div class="row" id="example-list"></div>
</div>

<script type="text/javascript">
  const examples = {{< readfile file="/static/data/tools_list.json" >}};

  let languageOptions = [`<option value="">All</option>`].concat(
    examples.map(e => `<option value="${e.language}">${e.language}</option>`));
  document.getElementById("filter-lang").innerHTML = languageOptions.join(' ');

  function updateExampleItems() {;
      const searchQuery = document.getElementById("search").value.trim().toLowerCase();
    const selectedLanguageFilter = document.getElementById("filter-lang").value;
    
    const exampleItems = examples.map(e => {
    let filteredLangExamples = e.examples;

if (searchQuery) {
        filteredLangExamples = filteredLangExamples.filter(example => example.name.toLowerCase().includes(searchQuery) ||
        example.description.toLowerCase().includes(searchQuery) || 
        example.tags.find(tag => tag.toLowerCase().includes(searchQuery)) ||
        e.tags.find(tag => tag.toLowerCase().includes(searchQuery)));
      }
if (selectedLanguageFilter) {
        filteredLangExamples = filteredLangExamples.filter(example => e.language === selectedLanguageFilter || 
          example.language === selectedLanguageFilter);
      }

const exampleCards = filteredLangExamples.map(example => {
const tags = e.tags.concat(example.tags).map(tagText => 
          `<span class="badge badge-pill badge-primary" style="line-height: 1.2; background-color: #9B9595; color: black;">${tagText}</span>`);

        return `<div class="col-xl-3 col-lg-4 col-md-6 col-12 my-3">
            <a class="card card-hover h-100 p-3" href="${example.link}" 
               style="user-select: text; box-shadow: 0 5px 10px 0 #9B9595; text-decoration: none;" draggable="false" target="_blank">
                <div>
                  <h4 class="px-2 mb-1 mt-4">${example.name}</h4>
                  <div class="px-2 mb-2">${example.description}</div>
                  <h5>${tags.join(' ')}</h5>
                </div>
            </a>
          </div>
        `
      });


return exampleCards.join(' ');
}).filter(card => card);    

    document.getElementById("example-list").innerHTML = exampleItems.length > 0 ?
      exampleItems.join(' ') : `<div class="col-12">No examples available</div>`;
  }
  updateExampleItems();
</script>
