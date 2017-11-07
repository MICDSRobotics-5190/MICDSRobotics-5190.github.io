---
layout: default
title: About Our Team
---

# {{ page.title }}

Technoramic is the co-ed high school robotics team from MICDS. We offer robotics after school, for four hours per week. Our team values good sportsmanship, proper self-conduct, honesty, and gracious professionalism. Many of our members have prior experience in FTC, while a few came to us this year with little to no experience at all. Regardless of grade and robotics background, our members have developed a passion for the team and have bonded immensely. We have an inexorable motivation and drive to win!

## Team Members

{% if site.data.team.members[0] %}
  <table>
  {% for group in site.data.team.members %}
    <tr>
      {% for item in group %}
      <td>
        <img src="{{ item.picture }}" alt="{{ item.name }}">
        <p> {{ item.name }} </p>
      </td>
      {% endfor %}
    </tr>
  {% endfor %}
  </table>
{% endif %}


#### Alumni

|:-------------------------:|:-------------------------:|:-------------------------:|
| Potato | Potata | Tomato |
