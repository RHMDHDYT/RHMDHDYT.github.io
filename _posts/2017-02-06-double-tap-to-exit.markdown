---
published: true
layout: post
title: 'UX Tips: Double tap back button untuk keluar aplikasi.'
date: {}
categories:
  - UI UX
description: Something about Lorem Ipsum
image: 'https://unsplash.it/2000/1200?image=133'
image-sm: 'https://unsplash.it/500/300?image=133'
---
Hi, ini postingan pertama gue dan gue mau sharing seputar _mobile UX_. Umumnya untuk menutup sebuah aplikasi digunakan _back button_ pada perangkat _gadget_, dan untuk menghindari ketidaksengajaan tombol _back_ tertekan maka biasanya ditambahkan sebuah _confirmation layout_ berbentuk _pop up_. Jika menggunakan _pop up dialog_, dari sisi _ux user_ diharuskan memindahkan jarinya dari _back button_ ke tengah layar dan ini tidak _thumb-friendly_. Nah, ada cara yang lebih efektif dan _thumb-friendly_ untuk membuat konfirmasi keluar aplikasi. Yaitu, _double tap back button_!



**Berikut ini contoh aplikasi yang menggunakan konsep _UX_ berbeda untuk keluar dari aplikasi.**

Zalora          |Bukalapak
:--------------------------:|:--------------------------:
![zalora](https://i.imgur.com/JCBct6il.jpg)  |   ![bukalapak](https://i.imgur.com/7Fa4iQcl.jpg)



Dengan menggunakan konsep double tap, user hanya perlu menggunakan jari di tempat yang sama. Lebih  _thumb-friendly_ bukan?. Nah sekarang kita coba implementasi di platform Android.

Sebuah activity pada android mempunyai default method untuk action onBackPressed. kita dapat menambahkan fungsi dengan memanggil method onBackPressed dari sebuah class yang meng-extend activity. Setelah itu tambahkan variable TIME_DELAY dan back_pressed dalam activity class.

```java
	private static final int TIME_DELAY = 2000;
    private static long back_pressed;
```
    
```java
	@Override
    public void onBackPressed() {
        if (back_pressed + TIME_DELAY > System.currentTimeMillis()) {
            super.onBackPressed();
        } else {
            Toast.makeText(getBaseContext(), "Press once again to exit!",
                    Toast.LENGTH_SHORT).show();
        }
        back_pressed = System.currentTimeMillis();
    }
```
