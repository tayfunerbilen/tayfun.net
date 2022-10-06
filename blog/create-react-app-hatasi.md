---
title: '"You are running create-react-app 4.0.3 which is behind the latest release (5.0.1)" Hatası'
date: '2022-08-16'
---

# "You are running create-react-app 4.0.3 which is behind the latest release (5.0.1)" Hatası

Eğer `create-react-app` ile bir proje oluştururken böyle bir hata alıyorsanız, bilin ki sürümünüz eskimiş. Şöyle kurmak yerine;

```shell
npx create-react-app .
```

sürüm belirterek kurarsanız güncel versiyonunu kurup işleme devam edecektir.

```shell
npx create-react-app@5.0.1 .
```

bir sorunun daha üstesinden geldik :D 
