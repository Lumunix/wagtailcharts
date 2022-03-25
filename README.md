# Wagtail Charts
Chart.js charts in Wagtail, edited and customised from the Wagtail admin

## Getting started

Assuming you have a Wagtail project up and running:

`pip install wagtailcharts`

Add `wagtailcharts` to your settings.py in the INSTALLED_APPS section, before the core wagtail packages:

```python
INSTALLED_APPS = [
    # ...
    'wagtailcharts',
    # ...
]
```

Add a wagtailcharts ChartBlock to one of your StreamFields:

```python
from wagtailcharts.blocks import ChartBlock

class ContentBlocks(StreamBlock):
    chart_block = ChartBlock()
```

Include your streamblock in one of your pages

```python
class HomePage(Page):
    body = StreamField(ContentBlocks())

    content_panels = Page.content_panels + [
        StreamFieldPanel('body'),
    ]
```

Add the `wagtailcharts_tags` templatetag to your template and call the `render_charts` tag just before your `</body>` closing tag.
Please note that you must render your chart block so that the `render_charts` tag can detect the charts.
Here is a tiny example of a page rendering template:

```django
{% load wagtailcore_tags wagtailcharts_tags %}

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

## Configuration

`ChartBlock` accepts a few extra arguments in addition to the standard `StructBlock` arguments.

### `colors`
A tuple of color tuples defining the available colors in the editor.

```python
from wagtailcharts.blocks import ChartBlock

COLORS = (
    ('#ff0000', 'Red'),
    ('#00ff00', 'Green'),
    ('#0000ff', 'Blue'),
)

class ContentBlocks(StreamBlock):
    chart_block = ChartBlock(colors=COLORS)
```

## Dependencies
This project relies on [Jspreadsheet Community Edition](https://bossanova.uk/jspreadsheet/v4/) for data entry and manipulation. Charts are rendered using [Chart.js](https://www.chartjs.org/).
