---
layout: page
permalink: /about/
title: About
tags: 
  - Neil
  - Chen
imagefeature: fourseasons.jpg
chart: true
charttype: pie
published: true
---

<figure>
  <!--img src="{{ site.url }}/images/page/hmfaysal-crest-square.jpg" alt="Hossain Mohammad Faysal"-->
  <figcaption>Neil Chen</figcaption>
</figure>

{% assign total_words = 0 %}
{% assign readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% if site.wpm %}{% assign indi_readtime = post_words | append: '.0' | divided_by:site.wpm %}{% else %}{% assign indi_readtime = post_words | append: '.0' | divided_by:180 %}{% endif %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign readtime = readtime | plus: indi_readtime %}
    {% if post.featured %}
    {% assign featuredcount = featuredcount | plus: 1 %}
    {% endif %}
{% endfor %}
{% for status in site.data.statuses %}{% assign statuscount = statuscount | plus:1 %}{% endfor %}

My name is **Neil Chen**, and this is my personal blog. It currently has {{ site.posts | size }} posts in {{ site.categories | size }} categories which combinedly have {{ total_words }} words, and will take an average reader ({{ site.wpm }} WPM) approximately {% if readtime > 60 %}{% assign readtime_hours = readtime | divided_by: 60 %}{% assign readtime_minutes = readtime | modulo:60 %}{% if readtime_hours > 1 and readtime_hours < 2 %}1 hour{% else %}<span class="hour">{{ readtime_hours }}</span> hours{% endif %}{% if readtime_minutes < 1 %}{% elsif readtime_minutes > 1 and readtime_minutes < 1.5 %} and 1 minute {% elsif readtime_minutes > 1.5 %} and <span class="time">{{ readtime_minutes }}</span> minutes{% endif %}{% else %}{% if readtime < 1 %}less than 1 minute {% elsif readtime > 1 and readtime < 1.5 %}1 minute {% elsif readtime > 1.5 %}<span class="time">{{ readtime }}</span> minutes {% endif %}{% endif %} to read. {% if featuredcount != 0 %}There are <a href="{{ site.url }}/featured">{{ featuredcount }} featured posts</a>, you should definitely check those out.{% endif %} The most recent post is {% for post in site.posts limit:1 %}{% if post.description %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}">"{{ post.title }}"</a>{% else %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}" title="Read more about {{ post.title }}">"{{ post.title }}"</a>{% endif %}{% endfor %} which was published on {% for post in site.posts limit:1 %}{% assign modifiedtime = post.modified | date: "%Y%m%d" %}{% assign posttime = post.date | date: "%Y%m%d" %}<time datetime="{{ post.date | date_to_xmlschema }}" class="post-time">{{ post.date | date: "%d %b %Y" }}</time>{% if post.modified %}{% if modifiedtime != posttime %} and last modified on <time datetime="{{ post.modified | date: "%Y-%m-%d" }}" itemprop="dateModified">{{ post.modified | date: "%d %b %Y" }}</time>{% endif %}{% endif %}{% endfor %}. The last commit was on {{ site.time | date: "%A, %d %b %Y" }} at {{ site.time | date: "%I:%M %p" }} [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time "Temps Universel Coordonné"). Check out the update log [here]({{ site.url }}/update-log).

Currently there are [{{ statuscount }} status messages.]({{ site.url }}/status-updates)

<div class="chart" id="chartdiv" style="width: 100%; height: 500px; margin-bottom: 20px;" ></div>



<!-- amCharts javascript code -->
<script type="text/javascript">
	AmCharts.makeChart("chartdiv",
		{
			"type": "pie",
			"pathToImages": "http://cdn.amcharts.com/lib/3/images/",
			"balloonText": "Category: [[title]]<br><span style='font-size:14px'><b>[[value]] Posts</b> ([[percents]]%)</span>",
			"innerRadius": "40%",
			"minRadius": 100,
			"pullOutRadius": "15%",
			"startRadius": "30%",
			"colors": [
				"#FC913A",
				"#F9D423",
				"#FF4E50",
				"#FCD202",
				"#F8FF01",
				"#B0DE09",
				"#04D215",
				"#0D8ECF",
				"#0D52D1",
				"#2A0CD0",
				"#8A0CCF",
				"#CD0D74",
				"#754DEB",
				"#DDDDDD",
				"#999999",
				"#333333",
				"#000000",
				"#57032A",
				"#CA9726",
				"#990000",
				"#4B0C25"
			],
			"hoverAlpha": 0.74,
			"pullOutEffect": "elastic",
			"pullOutOnlyOne": true,
			"startEffect": "easeOutSine",
			"titleField": "category",
			"valueField": "number-of-posts",
			"allLabels": [],
			"balloon": {},
			"legend": {
				"align": "center",
				"markerType": "diamond",
				"switchable": false,
				"textClickEnabled": true,
				"useMarkerColorForLabels": true,
				"useMarkerColorForValues": true,
				"valueText": "[[value]] Posts"
			},
			"titles": [
				{
					"id": "Title-1",
					"text": "Number of Posts Breakdown"
				}
			],
      "dataProvider": [
{% assign tags_list = site.categories %}  
  {% if tags_list.first[0] == null %}
    {% for tag in tags_list %} 
        {
          "category": "{{ tag | capitalize }}",
          "number-of-posts": {{ site.tags[tag].size }}
        },
    {% endfor %}
  {% else %}
    {% for tag in tags_list %} 
        {
          "category": "{{ tag[0] | capitalize }}",
          "number-of-posts": {{ tag[1].size }}
        },
    {% endfor %}
  {% endif %}
{% assign tags_list = nil %}
      ]
    }
  );
</script>