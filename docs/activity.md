---
layout: wiki
title: 活動の現在地
---
# 活動の現在地

アセットを別の環境で使うには、①道具同士が何を渡せば動くか、②利用者にどんな形で届けるか、の両方を決める必要があります。ここでは公開記録で裏付けられるところまでを示します。

{% for item in site.data.activity %}
## {{ item.title }}

**{{ item.status }}**（{{ item.checked }}）｜[確認に使った記事]({{ item.page }})

{{ item.result }}

次に確かめること：{{ item.next }}
{% endfor %}
