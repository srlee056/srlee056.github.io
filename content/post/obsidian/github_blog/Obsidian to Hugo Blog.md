---
tags:
  - obsidian_blog
author: Seorim
slug: obsidian-blog
categories:
  - Blog
title: Obsidian to Hugo Blog
date: 2024-04-30T02:46:00.633Z
lastmod: 2024-04-30T03:19:58.769Z
---
옵시디언으로 글 작성 후 Hugo Template로 바꿔서 post생성하는 방법!

hugo publish plugin을 사용하면 된다.\
![](/ob/github_blog/images/pasted_image_20240430120486.png)

blog를 위한 tag를 하나 무조건 설정해야한다는 단점?이 있지만,\
obsidian note -> github blog publish 를 바로 할 수 있다는 장점은 확실한 듯 하다.

![](/ob/github_blog/images/pasted_image_20240430120471.png)

이렇게 author, slug(uri), category를 설정해주면 (원래 Hugo blog config에 해당) 이에 맞게 변환된다\
이미지도 static 폴더 밑으로 알아서 복사해주고, path도 자동으로 수정해줌!

+) paste image rename plugin을 설치해서 기존에 `pasted image ...` 이렇게 공백으로 구분되어있는 이미지 이름을 `pasted_image_...` 언더바로 구분되게 변경했다. 공백으로 구분되는 경우에 `%20` 으로 변경하면 되지만, Hugo blog에선 왜인지 몰라도 인식이 안돼서 옵시디언 플러그인을 사용해서 수정했다.

![](/ob/github_blog/images/pasted_image_20240430120400.png)
