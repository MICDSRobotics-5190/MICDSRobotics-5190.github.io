---
layout: default
title: About Our Team
---

# {{ page.title }}

Technoramic is the co-ed high school robotics team from MICDS. We offer robotics after school for six hours per week. Our team values good sportsmanship, proper self-conduct, honesty, and gracious professionalism. Many of our members have prior experience in FTC, while a few came to us this year with little to no experience at all. Regardless of grade and robotics background, our members have developed a passion for the team and have bonded immensely. We have an inexorable motivation and drive to win!

## Team Members

{% for group in site.data.members %}
  <table style="border:0px;">
    {% for rows in group.people %}
    <tr style="border:0px;">
      {% for person in rows.row %}
      <td style="border:0px;">
        <img src="../{{ person.picture }}" alt="{{ person.name }}">
        <p> {{ person.name }} </p>
      </td>
      {% endfor %}
    </tr>
    {% endfor %}
  </table>
{% endfor %}


#### Alumni

{% for group in site.data.alumni %}
  <table style="border:0px;">
    {% for rows in group.people %}
    <tr style="border:0px;">
      {% for person in rows.row %}
      <td style="border:0px;">
        <img src="../{{ person.picture }}" alt="{{ person.name }}">
        <p> {{ person.name }} </p>
      </td>
      {% endfor %}
    </tr>
    {% endfor %}
  </table>
{% endfor %}
