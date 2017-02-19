---
published: false
layout: post
date: 2016-09-07T00:00:00.000Z
categories:
  - Marko
description: Something about Lorem Ipsum
image: 'https://unsplash.it/2000/1200?image=1047'
image-sm: 'https://unsplash.it/500/300?image=1047'
---
## Android: Measure time elapsed on function.
halo teman-teman, ini post pertama gue dan gue mau sharing tentang pengukuran waktu yang dibutuhkan dalam sebuah function / method dalam environtment Android.

Jadi gini, kadang kita mau mengukur berapa waktu yang dihabiskan pada suatu proses / fungsi dalam koding. Gunanya untuk apa? gunanya untuk mengoptimalkan proses didalamnya agar tidak memakan banyak waktu. Sederhananya, untuk mengukur waktu di dalam sebuah proses bisa menggunakan class System dengan fungsi nanoTime() pada java. Contoh simplenya seperti ini, kita simulasikan sebuah method melakukan sleep thread selama 3000ms: 

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

Di awal sebuah function / method kita ambil waktu dimulai dalam nanotime, kemudian di akhir proses kita ambil lagi waktu tersebut lalu tinggal dikurang dengan waktu dimulai tadi, simple kan?.
Setelah di tes, method ini mengeluarkan log berisi:
>02-19 18:06:21.477 6690-6690/com.rahmad.measuretime D/elapsed time:: 3003ms

Kenapa harus nanotime?. Didalam class System, kita bisa menggunakan currentTimeMillis() dan nanoTime() untuk mengambil current time, dua duanya bisa dipakai, tapi sebenernya dua fungsi ini punya tujuan yang berbeda. 
