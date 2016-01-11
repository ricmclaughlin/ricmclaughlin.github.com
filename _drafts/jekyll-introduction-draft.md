---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}






<h2 id="tdd-with-angularjs">TDD with AngularJS</h2>
<p>This project was the first one I have done with TDD. Nothing amazing but a good look into the logistics of getting a complete TDD stack going. Includes a bunch of TDD tools including <a href="http://chaijs.com/">chai</a>, <a href="https://mochajs.org/">mocha</a>, <a href="https://angular.github.io/protractor/#/">protractor</a> and <a href="https://gotwarlost.github.io/istanbul/">instanbul</a>. Lots of learning on this <a href="https://github.com/ricmclaughlin/ang_tdd">project</a>.</p>

<h2 id="rapid-wireframing-with-emmet-bootstrap-and-bootsketch">Rapid Wireframing with Emmet, Bootstrap and Bootsketch</h2>
<p>I’m not a fan of dead end deliverables… so making a low fidelity wireframe that turns into the real website is of huge interest to me. To that end I am working on making a rapid wireframing toolbox using 
<a href="http://getbootstrap.com">bootstrap</a>, <a href="http://emmet.io/">emmet</a> (the best html/css tool ever),  <a href="http://yago.github.io/Bootsketch/">bootsketch</a> (an low fidelity bootstrap theme designed for wireframes) and a bunch of snippets that will make it all faster than you thought possible. Still working on this one <a href="http://ric.mclaughlin.today/uibootstrap/#/">uibootstrap</a></p>

<h2 id="emmet">Emmet</h2>
<p>I wrote a blog post about this <a href="https://www.udemy.com/emmet-video-tutorials/">mini-course</a> for the best productivity enhancing editor plugin ever.  Please take this free course now! <a href="http://ric.mclaughlin.today/prj_html5_emmet/">Faster HTML &amp; CSS workflow with Emmet + Bootstrap</a></p>

<h1 id="walk-throughs">Walk Throughs</h1>
<p>I learn a lot by coding along while watching videos and reading books. This process really cements the knowledge for me. And I’ve done quite a few video series and books.. But it isn’t like these project come directly from the source material… I’ve modified and changed and improved every single project. Your results will vary.</p>

<h2 id="projects-in-html5">Projects in HTML5</h2>
<p>Tons of projects in the Udemy <a href="https://www.udemy.com/projects-in-html5/learn/#/">Projects in HTML5</a> course. Tons of cheese in there too… bad code formatting. Bad Boston accent. Clearly the instructor is a reformed php hack. But hey, we all come from somewhere.</p>

<p>+<a href="http://ric.mclaughlin.today/prj_html5_get_started/">Projects in HTML5 - Getting Started</a></p>

<p>+<a href="http://ric.mclaughlin.today/prj_html5_blog/">Projects in HTML5 - Blog Site</a></p>

<p>+<a href="http://ric.mclaughlin.today/prj_html5_gallery/">Projects in HTML5 - jQuery Image Gallery</a></p>

<h2 id="projects-in-bootstrap">Projects in Bootstrap</h2>
<p>Tons of projects in the Udemy <a href="https://www.udemy.com/projects-in-html5/learn/#/">Projects in HTML5</a> course. Simply a great review of Bootstrap - a bit tedious at times.</p>

<p>+<a href="http://ric.mclaughlin.today/prj_btstrp_photo_app/">Projects in Bootstrap - Photo App</a></p>

<p>+<a href="http://ric.mclaughlin.today/prj_btstrp_portfolio_sass/">Projects in Bootstrap - Portfolio Site with sass</a> </p>

<p>+<a href="http://ric.mclaughlin.today/prj_btstrp_dobble/">Projects in Bootstrap - Social Network site</a> </p>

<h2 id="projects-in-angularjs">Projects in AngularJS</h2>
<p>Just because I could not get enough of Brad Traversy I also have done quite a few Angularjs walkthrough videos. </p>



</article>


<div class="media">
  <div class="media-left">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Media heading</h4>
    ...
  </div>
</div>
<div class="container">
  <div class="row">
    <div class="col-md-2">
      <a href=""><h2 class="skillheader">Front End Dev</h2></a>
    </div>
    <div class="col-md-7">
      <a href=""><img class="techicon" src="/assets/themes/ricify/images/angular.png"></a>
      <a href=""><img class="techicon" src="/assets/themes/ricify/images/angular.png"></a>
      <a href=""><img class="techicon" src="/assets/themes/ricify/images/angular.png"></a>
    </div>
    <div class="col-md-2">
      <h3 class="skillheader">Projects <span class="badge">5</span> </h3>
    </div>
  </div>  
   
</div>

<!-- 
{% for project_tag in project_tags %}
    {% assign includer = "tags/" | append: project_tag | append: ".html" %}
    {% assign imager = "tags/" | append: project_tag | append: ".html" %}
    <div class="category-archive">
      {% include {{includer}} %}
      <ul class="posts">
        
        {% for post in site.categories[cat] %}
          {% if post.tags contains {{project_tag}} %}
            <ul> 
              <li><a href="{{ post.url }}">{{ post.title }}</a></li>
            </ul>
          {% endif %}  
        {% endfor %}
      </ul>
    </div>
  {% endfor %}   -->