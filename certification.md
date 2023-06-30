---
layout: page
title: My Certifications
categories: posts
---
{% assign project_tags = site.array %}
{% assign skills = site.otherarray %}
{% assign skills = "aws-solutions-arch-pro,aws-ml-spec,pmi-acp,pmp" | split: "," %}

  <ul class="nav nav-tabs" id="nav-tabs" role="tablist">
    {% for skill in skills %}
      {% if skill == "aws-ml-spec" %}
        {% assign skill_full_name = "AWS Machine Learning Specialty" %}
      {% elsif skill == "pmp" %}
        {% assign skill_full_name = "PMP" %}
      {% elsif skill == "pmi-acp" %}
        {% assign skill_full_name = "PMI - Agile" %}
      {% elsif skill == "aws-solutions-arch-pro" %}
        {% assign skill_full_name = "AWS Solutions Architect Pro" %}
      {% endif %}
      <li role="presentation">
        <a href="#{{skill}}" aria-controls="home" role="tab" data-toggle="tab">{{skill_full_name}}</a>
      </li>  
    {% endfor %} 
  </ul>


  <div class="tab-content">
    {% for skill in skills %}
      {% assign includer = "tags/" | append: skill | append: ".html" %}
      <div role="tabpanel" class="tab-pane" id="{{skill}}">
        <br>
        <p>{% include {{includer}} %}</p>
      </div>
    {% endfor %}
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
