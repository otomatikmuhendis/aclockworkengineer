---
icon: fas fa-code
order: 5
---

![Hacktoberfest Türkçe](/img/hacktoberfestBanner.png){: width="1200" height="720" }

Hackertoberfest katılımcıları

<ul id="ulattendeelist">
{% for att_hash in site.data.attendees %}
    {% assign attendee = att_hash[1] %}
    <li>
        {{ attendee.name }} <br/>  
        <img src="https://gravatar.com/avatar/?d=robohash" width="80" height="80" data-username="{{ attendee.username }}" data-domain="{{ attendee.domain }}"/>
    </li>
{% endfor %}
</ul>

<script defer>
function hash(string) {
  const utf8 = new TextEncoder().encode(string);
  return crypto.subtle.digest('SHA-256', utf8).then((hashBuffer) => {
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    const hashHex = hashArray
      .map((bytes) => bytes.toString(16).padStart(2, '0'))
      .join('');
    return hashHex;
  });
}

document.addEventListener('DOMContentLoaded', function () {
  const imgTags = document.querySelectorAll('#ulattendeelist img');

  imgTags.forEach((img) => {
    const email = img.dataset.username + '@' + img.dataset.domain;

    hash(email).then((hex) => img.src = "https://gravatar.com/avatar/"+hex+"?d=robohash");
  });
});
</script>