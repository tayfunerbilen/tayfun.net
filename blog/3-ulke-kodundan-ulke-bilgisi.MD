---
title: 'Ülke Kodundan Ülke bilgisi'
date: '2022-04-26'
---

# Ülke Kodundan Ülke bilgisi

Javascript'de `Intl` sınıfınds bir çok işe yarar metod var, bunlardan bir tanesi bugün işime yaradı. Elimde ülke kodu mevcut bir dizi var ve bu ülke kodlarına hangi ülke ve hangi dil olduğunu göstermek istiyordum.

Bunun için `Intl` altındaki `DisplayNames()` metodunu kullandım. Örneğin;

`en-TR`
`ar-SY`
`en-GB`
`en-US`

gibi değerler vardı elimde. Ve bunların karşılığında türkçe olarak değerlerini yazdırmak istedim. Bunları tek tek kontrol edip karşılıklarını yazdırabilirdim ama onun yerine `Intl` API'yi kullanarak çok daha efektik bir kod yazabildim, şöyle ki;

```js
const getNameByCountryCode = name => new Intl.DisplayNames(
  ['tr'], {
    type: 'language'
  }
)

console.log(getNameByCountryCode('en-TR')) // İngilizce (Türkiye)	
console.log(getNameByCountryCode('ar-SY')) // Arapça (Suriye)	
console.log(getNameByCountryCode('en-GB')) // İngiliz İngilizcesi	
console.log(getNameByCountryCode('en-US')) // Amerikan İngilizcesi	
```

Ayrıca `type` yerine `region`, `script` ya da `currency` yazarakta farklı sonuçlar alabilirdim.
Daha detaylı bilgi için [buraya](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DisplayNames) bakabilirsin.
