---
title: "Apa Itu React ?"
meta_title: "Apa Itu React ?"
description: "Apa Itu React ?"
date: 2022-01-10T05:00:00Z
image: "/images/blog/what-is-react.png"
categories: ["Technology"]
author: "Purnama Anaking"
tags: ["menikah", "sunnah", "nazhor", "ta'aruf"]
draft: false
---

**React** adalah library javascript untuk membangun sebuah user interface (antarmuka). That’s it. Ok. Got it. Tiga hal tentang react yang perlu diketahui di awal belajar, yaitu dia (React) itu _Declarative_, _Component-Based_, dan _Learn Once, Write Anywhere_.

#### 1. React itu Declarative

_Declarative_ adalah lawan dari _Imperative_, Sebagai gambaran, _Imperative_ itu seperti ketika ingin makan lalu kita akan bilang kepada seorang koki,

> Pergilah ke dapur, buka kulkas, ambil ayam dari kulkas, masak, … , bawa hasil masakan ke atas meja.

Sedangkan _Declarative_ itu seperti kita ingin makan lalu kita cukup bilang kepada seorang koki,

> Saya mau makan malam dengan ayam.

Dengan React yang _Declarative_ kita bisa lebih mudah dalam membangun sebuah UI yang interaktif. Poin penting tentang _Declarative_ adalah kita bisa membuat desain tampilan untuk setiap _state_ di dalam aplikasi kita, dan React akan secara efisien _meng-update_ dan _me-render_ komponen yang sesuai, ketika _state_ atau datanya mengalami perubahan. Pendekatan _Declarative_ akan membuat kode kita lebih terprediksi dan lebih mudah untuk _di-debug_.

By the way, perbedaan pendekatan _Declarative_ dan _Imperative_ bisa kita lihat pada ilustrasi berikut ini.

Misal, kita ingin membuat komponen UI, yaitu **Like Button**. Jika sebelumnya warna button-nya adalah abu-abu maka ketika kita klik warnanya akan menjadi biru, sedangkan jika sebelumnya warna button-nya adalah biru maka ketika kita klik warnanya menjadi abu-abu.

##### Pendekatan Imperative:

```
if( user.likes() ) {
    if( hasBlue() ) {
        removeBlue();
        addGrey();
    } else {
        removeGrey();
        addBlue();
    }
}
```

##### Pendekatan Declarative:

```
if( this.state.liked ) {
    return <blueLike />;
} else {
    return <greyLike />;
}
```

Nah, bisa kita lihat bahwa dengan pendekatan _Declarative_ kita hanya perlu meng-handle bagaimana seharusnya sebuah UI yang ditampilkan pada sebuah _state_ (kondisi), sehingga dengan begitu mengembangkan sebuah komponen UI menjadi lebih sederhana dan mudah untuk dipahami.

#### 2. React itu Component-Based

Dengan React kita membangun sebuah UI / antar muka dengan pendekatan komponen. Sebuah UI yang kompleks dibangun dari gabungan sekian komponen yang ada. Komponen tersebut ter-enkapsulasi dan memiliki _state_ untuk dirinya masing-masing.

#### 3. React itu Learn Once, Write Anywhere

Sebagai _UI library javascript_, React dapat dipasangkan dengan teknologi apa saja. React dapat di-render pada sisi server menggunakan _Node_ dan juga dapat mendukung pengembangan aplikasi mobile menggunakan teknologi _React Native_.
