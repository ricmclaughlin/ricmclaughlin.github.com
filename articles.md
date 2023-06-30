---
layout: page
title: Articles
categories: projects
---

{% assign cat = 'delivery' %}
{% assign project_tags = site.array %}
{% assign skills = site.otherarray %}
{% assign skills = "aws,solution-delivery,ml" | split: "," %}

<div>
  <!-- Nav tabs -->
  <ul class="nav nav-tabs" id="nav-tabs" role="tablist">
    {% for skill in skills %}
      {% if skill == "aws" %}
        {% assign skill_full_name = "Solution Architecture" %}
      {% elsif skill == "ml" %}
        {% assign skill_full_name = "Machine Learning" %}
      {% elsif skill == 'solution-delivery' %}
        {% assign skill_full_name = "Solution Delivery" %}
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
          {% if skill == "ml" %}
            {% assign project_tags = "ml-ethics-safety,design-data,modeling,mlops,ai-services" | split: "," %}
          {% elsif skill == 'solution-delivery' %}
            {% assign project_tags = "special-sauce,scrum,kanban,leanux" | split: "," %}
          {% elsif skill == 'aws' %}
            {% assign project_tags = "aws-system-design,aws-guides,mlops,analytics,devops,storage,compute,networking,database,integration,management,other,serverless,media-srv" | split: "," %}
          {% endif %}

          {% assign includer = "tags/" | append: skill | append: ".html" %}
          {% assign portfolio_skill_ref = skill | append: "-ref" %}
          <br>
          <p>{% include {{includer}} %}</p>
            {% for tag in project_tags %}
            {% if tag == "aws-system-design" %}
              {% assign tag_full_name = "System Design using AWS" %}
            {% elsif tag == 'aws-guides' %}
              {% assign tag_full_name = "SA Pro Topics" %}
            {% elsif tag == 'special-sauce' %}
              {% assign tag_full_name = "Ric's Special Sauce" %}
            {% elsif tag == 'ml-ethics-safety' %}
              {% assign tag_full_name = "AI Ethics and Safety" %}
            {% elsif tag == 'design-data' %}
              {% assign tag_full_name = "ML Data Design" %}
            {% elsif tag == 'ai-services' %}
              {% assign tag_full_name = "AI Services" %}
            
            {% elsif tag == 'serverless' %}
              {% assign tag_full_name = "AWS Serverless Offerings" %}
            {% elsif tag == 'media-srv' %}
              {% assign tag_full_name = "Media Services" %}
            {% elsif tag == 'management' %}
              {% assign tag_full_name = "Management Services (a catch-all)" %}
            {% elsif tag == 'mlops' %}
              {% assign tag_full_name = "Machine Learning Operations" %}
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
                <h3 class="media-heading" >
                  <a href="{{tag_page_ref}}">{{tag_full_name | capitalize}}</a>
                </h3>
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
  var tabToActivate = window.location.hash || '#aws';
  $('#nav-tabs a[href="' + tabToActivate + '"]').tab('show')
  $('a[data-toggle="tab"]').on('click', function(e) {
    history.pushState(null, null, $(this).attr('href'));
  });

  window.addEventListener("popstate", function(e) {
    var tabToActivate = window.location.hash || '#aws';
    $('#nav-tabs a[href="' + tabToActivate + '"]').tab('show')
  }); 
});

</script>



