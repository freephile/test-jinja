

The special 'comment' directive is only valid as the first line in the template file
The DEFAULT for Jinja in Ansible is
#jinja2: trim_blocks: "true", lstrip_blocks: "false"

You can test the output of this template directly with
ansible all -i "localhost," -c local -m template -a "src=./test-jinja/templates/manual_blocks.j2 dest=./test-jinja/files/manual_blocks.md"

## Raw
This is the jinja block we're using throughout this template, slightly
modified in each variant.

{% set foo="default" %}
{% for i in [1, 2, 3] %}
    {% if loop.first %}
      {{i}} {{foo}}
    {% else %}
      {{i}} {{foo}}
    {% endif %}
{% endfor %}


## Default
Here we just have the 'control' with no manual adjustments
      1 default
      2 default
      3 default


## Using plus (+)
Disable `lstrip_blocks` with a + at the beginning

{% set bar="using plus" %}
{% for i in [1, 2, 3] %}
    {%+ if loop.first %}
        {{i}} {{bar}}
    {% else %}
        {{i}} {{bar}}
    {% endif %}
{% endfor %}
            1 using plus
            2 using plus
            3 using plus



Disable `trim_blocks` with a + at the end
Unfortunately, this resulted in a parse error in my version of Ansible/Jinja

{% set bar="using plus" %}
{% for i in [1, 2, 3] %}
    {%+ if loop.first +%}
        {{i}} {{bar}}
    {% else %}
        {{i}} {{bar}}
    {% endif %}
{% endfor %}



## Using minus (-)
You can also strip whitespace in templates by hand. 
If you add a minus sign (-) to the start or end of a block (e.g. a For tag), a comment, or a variable expression, 
the whitespaces before or after that block will be removed:

Using minus before the else

{% set foo="use minus" %}
{% for i in [1, 2, 3] %}
    {% if loop.first %}
        {{i}} {{foo}}
    {%- else %}
        {{i}} {{foo}}
    {% endif %}
{% endfor %}
        1 use minus        2 use minus
        3 use minus

Use minus after the else

{% set foo="use minus" %}
{% for i in [1, 2, 3] %}
    {% if loop.first %}
        {{i}} {{foo}}
    {%else -%}
        {{i}} {{foo}}
    {% endif %}
{% endfor %}
        1 use minus
2 use minus
3 use minus


Use minus after the 'if', and 'else'

{% set foo="use minus" %}
{% for i in [1, 2, 3] %}
    {% if loop.first -%}
        {{i}} {{foo}}
    {%else -%}
        {{i}} {{foo}}
    {% endif %}
{% endfor %}
1 use minus
2 use minus
3 use minus


Use minus before and after only the 'else'

{% set foo="use minus" %}
{% for i in [1, 2, 3] %}
    {% if loop.first %}
        {{i}} {{foo}}
    {%- else -%}
        {{i}} {{foo}}
    {% endif %}
{% endfor %}
        1 use minus2 use minus
3 use minus

Use minus before and after the 'if', 'else', 'endif'

{% set foo="use minus" %}
{% for i in [1, 2, 3] %}
    {%- if loop.first -%}
        {{loop.index}} {{foo}}
    {%- else -%}
        {{loop.index}} {{foo}}
    {%- endif -%}
{% endfor %}
1 use minus2 use minus3 use minus

