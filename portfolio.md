---
layout: page
title: Portfolio
categories: projects
---

{% assign cat = 'delivery' %}
{% assign project_tags = site.array %}
{% assign skills = site.otherarray %}
{% assign skills = "aws,web-dev,devops,data,languages,project-delivery,curriculum-dev" | split: "," %}

<div>
  <!-- Nav tabs -->
  <ul class="nav nav-tabs" id="nav-tabs" role="tablist">
    {% for skill in skills %}
      {% if skill == "web-dev" %}
        {% assign skill_full_name = "Web Development" %}
      {% elsif skill == "devops" %}
        {% assign skill_full_name = "DevOps" %}
      {% elsif skill == "aws" %}
        {% assign skill_full_name = "AWS" %}
      {% elsif skill == 'data' %}
        {% assign skill_full_name = "Data" %}
     {% elsif skill == 'languages' %}
        {% assign skill_full_name = "Languages" %}
      {% elsif skill == 'curriculum-dev' %}
        {% assign skill_full_name = "Curriculum Development" %}
      {% elsif skill == 'project-delivery' %}
        {% assign skill_full_name = "Project Delivery" %}
      {% endif %}

      <li role="presentation">
        <a href="#{{skill}}" aria-controls="home" role="tab" data-toggle="tab">{{skill_full_name}}</a>
      </li>
    {% endfor %} 
  </ul>

  <!-- Tab panes -->
  <div class="tab-content">
    {% for skill in skills %}
      <div role="tabpanel" class="tab-pane" id="{{skill}}">
        <div class="category-archive">
          {% if skill == "web-dev" %}
            {% assign project_tags = "node,express,reactjs,html-css,mongodb" | split: "," %}
          {% elsif skill == "devops" %}
            {% assign project_tags = "cloudformation,git,travis,opsworks,elastic-beanstalk" | split: "," %}
          {% elsif skill == 'data' %}
            {% assign project_tags = "dynamodb,redshift,elasticache,rds,mongodb" | split: "," %}
          {% elsif skill == 'languages' %}
            {% assign project_tags = "javascript,ruby,python" | split: "," %}
          {% elsif skill == 'classes' %}
            {% assign project_tags = "portfoliodev,htmlcssclass,javascriptclass,c9" | split: "," %}
          {% elsif skill == 'curriculum-dev' %}
            {% assign project_tags = "iot-aws,data-struct,portfolio-dev" | split: "," %}
          {% elsif skill == 'project-delivery' %}
            {% assign project_tags = "special-sauce,scrum,kanban,leanux" | split: "," %}
          {% elsif skill == 'aws' %}
            {% assign project_tags = "aws" | split: "," %}
          {% endif %}

          {% assign includer = "tags/" | append: skill | append: ".html" %}
          {% assign portfolio_skill_ref = skill | append: "-ref" %}
          <br>
          <p>{% include {{includer}} %}</p>
          
          
          {% for tag in project_tags %}
            {% if tag == 'basicfront' %}
              {% assign tag_full_name = "Front End Development" %}
            {% elsif tag == 'basicback' %}
              {% assign tag_full_name = "Backend End Development" %}
            {% elsif tag == 'tdd' %}
              {% assign tag_full_name = "Test Driven Development" %}
            {% elsif tag == 'api' %}
              {% assign tag_full_name = "API Development" %}
            {% elsif tag == 'webapp' %}
              {% assign tag_full_name = "Web Applications" %}
            {% elsif tag == 'htmlcss' %}
              {% assign tag_full_name = "HTML5 & CSS" %}
            {% else %}
              {% assign tag_full_name = {{tag | capitalize}}  %}
            {% endif %}

            {% assign image_url = "/assets/themes/ricify/images/" | append: tag | append: ".png" %}
            {% assign includer = "tags/" | append: tag | append: ".html" %}
            {% assign tag_page_ref = "/tags.html#" | append: tag %}
            
            <div class="media" id="{{portfolio_tag_ref}}">
              <div class="media-left">
                <a href="{{tag_page_ref}}">
                  <img class="media-object" src="{{image_url}}" alt="{{tag_full_name | capitalize}} logo">
                </a>
              </div>
              <div class="media-body">
                <h3 class="media-heading" ><a href="{{tag_page_ref}}">{{tag_full_name | capitalize}}</a></h3>
                {% if tag != skill %}
                  <p>{% include {{includer}} %}</p>
                {% endif %}
                {% assign cat = 'posts' %}
                  {% for post in site.categories[cat] %}
                    {% if post.tags contains tag %}
                      {% assign counter = 1 %}
                    {% endif %}
                  {% endfor %}
                  
                  {% if counter == 1%}
                    <p>{{tag_full_name }} Posts:</p>
                    <ul class="posts">
                    {% for post in site.categories[cat] %}
                      {% if post.tags contains tag %}
                        <li><a href="{{ post.url }}">{{ post.title }}</a></li> 
                      {% endif %}
                    {% endfor %}
                    </ul>
                  {% endif %}
                  
                  {% assign cat = 'projects' %}
                  {% assign counter = 0 %}
                    {% for post in site.categories[cat] %}
                    {% if post.tags contains tag %}
                      {% assign counter = 1 %}
                    {% endif %}
                  {% endfor %}
                  
                  {% if counter == 1%}
                    <p>{{ tag_full_name }} Projects:</p>
                    <ul class="posts">
                    {% for post in site.categories[cat] %}
                      {% if post.tags contains tag %}
                        <li><a href="{{ post.url }}">{{ post.title }}</a></li> 
                      {% endif %}
                    {% endfor %}
                    </ul>
                  {% endif %}
                  
                  {% assign counter = 0 %}
                </div>
            </div>
            

            {% endfor %}
        </div>
      </div>
    {% endfor %} 
  </div>

</div>
<div id="footerbar"></div>

<script>
$( document ).ready(function() {
  var parseQueryString = function( queryString ) {
    var params = {}, queries, temp, i, l;
    // Split into key/value pairs
    queries = queryString.split("&");
    // Convert the array of strings into an object
    for ( i = 0, l = queries.length; i < l; i++ ) {
        temp = queries[i].split('=');
        params[temp[0]] = temp[1];
    }
    return params;
  };
  var queryStringObject = parseQueryString(window.location.search.substr(1));
  var tabToActivate = queryStringObject.t || 'aws';
  $('#nav-tabs a[href="#' + tabToActivate + '"]').tab('show')
  if (queryStringObject.c) {
    $('#' + queryStringObject.c).scrollTo();
  }
  // $('a[data-toggle="tab"]').on('click', function(e) {
  //   history.pushState(null, null, $(this).attr('href'));
  // });
  // // navigate to a tab when the history changes
  // window.addEventListener("popstate", function(e) {
  //   var activeTab = $('[href=' + location.hash + ']');
  //   if (activeTab.length) {
  //     activeTab.tab('show');
  //   } else {
  //     $('.nav-tabs a:first').tab('show');
  //   }
  // }); 
});

</script>



