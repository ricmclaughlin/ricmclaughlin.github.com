---
layout: page
title: Certification
categories: posts
---
{% assign project_tags = site.array %}
{% assign skills = site.otherarray %}
{% assign skills = "aws-solutions-arch-pro,aws-dev-ops-pro,pmp,pmi-acp" | split: "," %}

<div>
  <ul class="nav nav-tabs" id="nav-tabs" role="tablist">
    {% for skill in skills %}
      {% if skill == "aws-dev-ops-pro" %}
        {% assign skill_full_name = "AWS DevOps Engineer Pro" %}
      {% elsif skill == "aws-solutions-arch-pro" %}
        {% assign skill_full_name = "AWS Solutions Architect Pro" %}
      {% elsif skill == "pmp" %}
        {% assign skill_full_name = "PMP" %}
      {% elsif skill == "pmi-acp" %}
        {% assign skill_full_name = "PMI - Agile" %}
      {% elsif skill == "aws-academy" %}
        {% assign skill_full_name = "AWS Academy Accredited Instructor" %}
      {% endif %}


      <li role="presentation">
        <a href="#{{skill}}" aria-controls="home" role="tab" data-toggle="tab">{{skill_full_name}}</a>
      </li>
    {% endfor %} 
  </ul>


  <div class="tab-content">
    {% for skill in skills %}
      <div role="tabpanel" class="tab-pane" id="{{skill}}">
        <div class="category-archive">
          {% if skill == "aws-dev-ops-pro" %}
            {% assign project_tags = "cloudformation,opsworks,elastic-beanstalk,cloudwatch" | split: "," %}
            
          {% elsif skill == 'aws-solutions-arch-pro' %}
            {% assign project_tags = "aws-guides,cloudfront,rds,kinesis,iam,vpc,cloudformation,opsworks,elastic-beanstalk,cloudwatch,dynamodb,cloudsearch,aws-services" | split: "," %}
            
          {% else %}
            {% assign project_tags = '' %}
          {% endif %}

          {% assign includer = "tags/" | append: skill | append: ".html" %}
          {% assign portfolio_skill_ref = skill %}
          <br>
          <p>{% include {{includer}} %}</p>
 
          {% for tag in project_tags %}
            {% if tag == 'pmp' %}
              {% assign tag_full_name = "Project Management Professional" %}
            {% elsif tag == 'pmi-acp' %}
              {% assign tag_full_name = "Project Management Institute Agile Certified Practioner" %}
            {% elsif tag == 'aws-academy' %}
              {% assign tag_full_name = "AWS Academy Accredited Instructor" %}
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

<script>
$( document ).ready(function() {
  var tabToActivate = window.location.hash || '#aws-solutions-arch-pro';
  $('#nav-tabs a[href="' + tabToActivate + '"]').tab('show')
  $('a[data-toggle="tab"]').on('click', function(e) {
    history.pushState(null, null, $(this).attr('href'));
  });

  window.addEventListener("popstate", function(e) {
    var tabToActivate = window.location.hash || '#aws-solutions-arch-pro';
    $('#nav-tabs a[href="' + tabToActivate + '"]').tab('show')
  }); 
});

</script>

<div id="footerbar"></div>
