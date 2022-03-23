# Wagtail Charts
Chart.js charts in Wagtail, edited and customised from the Wagtail admin

## Getting started

Assuming you have a Wagtail project up and running:

`pip install wagtailcharts`

Add `wagtailcharts` to your settings.py in the INSTALLED_APPS section, before the core wagtail packages:

```
INSTALLED_APPS = [
    # ...
    'commonblocks',
    'wagtail.contrib.wagtailsitemaps',
    # ...
]
```

Add a wagtailcharts ChartBlock to one of your StreamFields:

```
from wagtail_charts.blocks import CartesianChartBlock as ChartBlock

class ContentBlocks(StreamBlock):
    chart_block = ChartBlock()
```

Add the ... templatetag to your template and call the `render_charts` tag just before your `</body>` closing tag.
Please note that you must render your chart block so that the `render_charts` tag can detect the charts.
Here is a tiny example of a page rendering template:

```
{% load wagtailcore_tags wagtail_charts_tags %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <div class="col-6">
            <h1>{{self.title}}</h1>
            <div class="excerpt">{{self.excerpt|richtext}}</div>
        </div>
    </div>
    {% for block in self.body %}
        {% include_block block %}
    {% endfor %}
</div>
{% endblock %}

{% block extra_js %}
{% render_charts %}

{% endblock %}
```


