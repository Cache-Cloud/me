---
title: 友情链接
date: 2025-01-26 16:51:34
type: "links"
---
<!DOCTYPE html>
<html lang="zh-CN">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="/links/styles.css">
  <title>友情链接</title>
</head>

<body>
  <div class="links-container" id="links-container"></div>
  
  <script>
    // 友情链接数据
    const linksData = [
      {
        href: "https://www.yuanshen.dev/",
        imgSrc: "https://www.yuanshen.dev/img/nahida.png",
        altText: "LINUX DO",
        fallbackImgSrc: "/links/YuanRetro/nahida.png",
        title: "YuanRetro",
        info: "一个二刺螈，无线电火腿，交通爱好者和科技菌的小站~"
      },
      {
        href: "https://bellanilla.neocities.org/",
        imgSrc: "/links/bellanilla/bellanilla.webp",
        altText: "Belladonna",
        fallbackImgSrc: "/links/bellanilla/bellanilla.webp",
        title: "Belladonna",
        info: "A unique Victorian style website"
      }
    ];

    // 错误处理函数
    function handleImageError(imgElement, fallbackSrc) {
      imgElement.src = fallbackSrc;
    }

    // 生成链接项的HTML
    function createLinkItem(link) {
      const linkItem = document.createElement('div');
      linkItem.classList.add('link-item');

      const linkCard = document.createElement('a');
      linkCard.href = link.href;
      linkCard.target = '_blank';
      linkCard.classList.add('link-card');

      const linkIcon = document.createElement('div');
      linkIcon.classList.add('link-icon');

      const img = document.createElement('img');
      img.src = link.imgSrc;
      img.alt = link.altText;
      img.onerror = () => handleImageError(img, link.fallbackImgSrc);
      img.width = 80;
      img.height = 80;

      const linkInfo = document.createElement('div');
      linkInfo.classList.add('link-info');

      const title = document.createElement('h3');
      title.textContent = link.title;

      const info = document.createElement('p');
      info.textContent = link.info;

      linkIcon.appendChild(img);
      linkInfo.appendChild(title);
      linkInfo.appendChild(info);
      linkCard.appendChild(linkIcon);
      linkCard.appendChild(linkInfo);
      linkItem.appendChild(linkCard);

      return linkItem;
    }

    // 渲染链接列表
    function renderLinks() {
      const container = document.getElementById('links-container');
      linksData.forEach(link => {
        const linkItem = createLinkItem(link);
        container.appendChild(linkItem);
      });
    }

    // 页面加载完成后渲染链接列表
    window.addEventListener('load', renderLinks);
  </script>
</body>

</html>
