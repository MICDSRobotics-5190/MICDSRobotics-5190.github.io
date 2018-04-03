---
layout: default
title: Our Sponsors
---

# {{ page.title }}

Technoramic is very happy to have the fantastic support of many wonderful sponsors. Without them, we wouldn't be able to do all that we can today. From everyone at Technoramic, thank you!

## Sponsors

{% for group in site.data.sponsors %}
  <table style="border:0px;">
    {% for rows in group.companies %}
    <tr style="border:0px;">
      {% for company in rows.row %}
      <td style="border:0px;">
        <a href="{{ company.link}}"><img src="../{{ company.picture }}" alt="{{ company.name }}"></a>
        <!--p> {{ company.name }} </p-->
      </td>
      {% endfor %}
    </tr>
    {% endfor %}
  </table>
{% endfor %}
