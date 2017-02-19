---
published: false
title: 'Android: Measure time elapsed on function.'
layout: post
date: 2017-02-19
categories:
  - Android
description: 'Android: Measure time elapsed on function.'
image: 'https://i.imgur.com/pyZbYcYl.jpg'
image-sm: 'https://i.imgur.com/pyZbYcYt.jpg'
---

Halo teman-teman, ini post pertama gue dan gue mau sharing tentang pengukuran waktu yang dibutuhkan dalam meng-eksekusi sebuah function / method dalam environtment Android.

Jadi gini, kadang kita mau mengukur berapa waktu yang dihabiskan pada peng-eksekusian suatu proses / fungsi dalam koding. Gunanya untuk apa? gunanya untuk mengoptimalkan proses didalamnya agar tidak memakan banyak waktu. Sederhananya, untuk mengukur waktu di dalam sebuah proses bisa menggunakan class System dengan fungsi nanoTime() pada java. Contoh simplenya seperti ini, kita simulasikan sebuah method melakukan sleep thread selama 3000ms: 

{% highlight java linenos %}
private void doSomething() {
    long tStart = System.nanoTime();

    try {
      Thread.sleep(3000);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }

    long tEnd = System.nanoTime();
    long tRes = tEnd - tStart;
    long mili = TimeUnit.NANOSECONDS.toMillis(tRes);

    Log.d("elapsed time: ", String.valueOf(mili) + "ms");
  }
{% endhighlight %}
<br/>
Di awal sebuah function / method, kita ambil waktu dimulai (current time) dalam nanotime kedalam sebuah variabel (tStart), kemudian di akhir proses kita ambil lagi (current time) kedalam variabel (tEnd), lalu variabel akhir tinggal dikurangi dengan variabel awal, simple kan?.
Setelah di tes, method ini akan mengeluarkan log berisi waktu (dalam miliseconds) yang digunakan untuk menyelesaikan satu proses diatas seperti ini:
>02-19 18:06:21.477 6690-6690/com.rahmad.measuretime D/elapsed time:: 3003ms

Kenapa harus nanoTime()?. Didalam class System, kita bisa menggunakan currentTimeMillis() dan nanoTime() untuk mengambil current time, dua-duanya bisa dipakai, tapi sebenernya dua fungsi ini punya tujuan yang berbeda.
currentTimeMillis() mengambil current time dari date operating system yang berjalan, kenapa method ini tidak disarankan? karena keakuratan jam pada os bisa bergeser dan harus diatur terus-menerus sehingga bisa mengurangi keakuratan perhitungan antara start time dan end time. lebih lanjut dari method currentTimeMillis() bisa dilihat [disini.](https://developer.android.com/reference/java/lang/System.html#currentTimeMillis())

Sedangkan method nanoTime() memang tujuan fungsinya adalah untuk mengukur sebuah waktu (elapsed time) dan tidak ada hubungannya dengan datetime OS sehingga lebih akurat. lebih lanjut dari method nanoTime() bisa dilihat [disini.](https://developer.android.com/reference/java/lang/System.html#nanoTime())

## Alternative way to measure elapsed time: Hugo
