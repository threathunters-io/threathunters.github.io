---
title: threathunter.io
---

{{< blocks/cover height="full" color="primary" >}}
<div style="display: flex; align-items: center;">
    <div style="flex: 1; margin-right: 20px;">
        <img src="images/logo_transparent.png" alt="Description of the image" style="max-width: 80%; height: auto;">
    </div>
    <div style="flex: 1; text-align: justify;">
        <h2>Community for Security Experts</h2>
        <p>Technical community to exchange knowledge with other security experts, focusing on enhancing cybersecurity measures and mitigating risks.</p>
    </div>
</div>

{{< blocks/link-down color="info" >}}
{{< /blocks/cover >}}


{{% blocks/section color="white" type="container" %}}
<div class="container">
    <div class="row" id="example-list"></div>
</div>

<script type="text/javascript">
  const examples = {{< readfile file="/static/data/feature_list.json" >}};

  function updateExampleItems() {
    const exampleItems = examples.map(e => {
      let filteredLangExamples = e.examples;

      const exampleCards = filteredLangExamples.map(example => {
        const tags = e.tags.concat(example.tags).map(tagText => 
          `<span class="badge badge-pill badge-primary" style="line-height: 1.2; background-color: #9B9595; color: black;">${tagText}</span>`);

        return `<div class="col-lg-20">
                  <a class="card card-hover h-100 p-3" href="${example.link}" style="user-select: text; box-shadow: 0 5px 10px 0 #9B9595; text-decoration: none;" draggable="false" target="_blank">
                    <div style="flex: 0 0 50%; display: flex; align-items: top; padding: 10px;">
                      <i class="${example.icon}" style="font-size: 50px; margin-right: 30px;"></i>
                      <div style="flex: 1; text-align: justify;">
                        <h2>${example.name}</h2>
                        <p>${example.description}</p>
                      </div>
                    </div>               
                  </a>
                </div>
              `;
      });

      return exampleCards.join(' ');
    }).filter(card => card);

    document.getElementById("example-list").innerHTML = exampleItems.length > 0 ?
      exampleItems.join(' ') : `<div class="col-12">No examples available</div>`;
  }

  updateExampleItems();
</script>
{{% /blocks/section %}}

{{% blocks/section color="dark" type="row" %}}

{{% blocks/feature icon="fab fa-github" title="Follow us on GitHub!" url="https://github.com/threathunters-io" url_text="GitHub" %}}
We do a [Pull Request](https://github.com/google/docsy-example/pulls) contributions workflow on **GitHub**. New users are always welcome!
{{% /blocks/feature %}}

{{% blocks/feature icon="fab fa-twitter" title="Follow us on Twitter!" url="https://twitter.com/threathuntersio" url_text="Twitter" %}}
For announcement of latest news etc.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-regular fa-envelope" title="Get in Touch!" url="mailto:your@email.com" url_text="Mail" %}}
{{% /blocks/feature %}}

{{% /blocks/section %}}
