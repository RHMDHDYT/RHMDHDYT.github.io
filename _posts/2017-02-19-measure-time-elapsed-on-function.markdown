---
published: true
title: 'Android: Measure time elapsed on function.'
layout: post
date: 2017-02-19
categories:
  - Android
description: 'Android: Measure time elapsed on function.'
image: 'https://i.imgur.com/pyZbYcYh.jpg'
image-sm: 'https://i.imgur.com/pyZbYcYl.jpg'
---

Halo teman-teman, ini post pertama gue dan gue mau sharing tentang cara mengukur waktu yang dibutuhkan ketika sebuah *function / method* di eksekusi.

Kadang kita perlu mengukur berapa waktu yang dihabiskan ketika mengeksekusi sebuah *function* untuk mem-*benchmark* seberapa lama prosesnya. Sederhananya, untuk mengukur waktu bisa dengan cara mengambil waktu saat proses dimulai dan berakhir menggunakan *class System* dengan fungsi nanoTime() pada java.

Contoh sederhananya seperti ini, kita simulasikan sebuah *method* melakukan *sleep thread* selama 3000ms. Berapa waktu yang dihabiskan ?, mari kita praktekkan: 

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

Di awal sebuah proses *function* dimulai, kita ambil waktu *(current time)* dalam *nano time*, kemudian di akhir proses kita ambil lagi *(current time)* nya, lalu tinggal mengurangi waktu akhir dengan waktu awal, *simple* kan?.

Setelah di tes, *function* ini memakan waktu selama 3003ms saat di eksekusi seperti log dibawah ini:
{% highlight java %}
02-19 18:06:21.477 6690-6690/com.rahmad.measuretime D/elapsed time:: 3003ms
{% endhighlight %}
<br/>

### Tambahan :

Kenapa harus *nanoTime()*?. Didalam *class System*, umumnya kita bisa menggunakan *currentTimeMillis()* dan *nanoTime()* untuk mengambil *current time*, dua-duanya bisa dipakai, tapi sebenarnya dua fungsi ini punya tujuan yang berbeda.

*currentTimeMillis()* mengambil *current time* dari *date operating system* yang berjalan, kenapa *method* ini tidak disarankan? karena keakuratan jam pada *system* bisa bergeser dan harus diatur terus-menerus sehingga bisa mengurangi keakuratan perhitungan antara *start time* dan *end time*. Lebih lanjut dari *method currentTimeMillis()* bisa dilihat [**disini.**](https://developer.android.com/reference/java/lang/System.html#currentTimeMillis())

Sedangkan *method nanoTime()* memang tujuan fungsinya adalah untuk mengukur sebuah waktu *(elapsed time)* dan tidak ada hubungannya dengan *datetime* *system* sehingga lebih akurat. lebih lanjut dari *method nanoTime()* bisa dilihat [**disini.**](https://developer.android.com/reference/java/lang/System.html#nanoTime())
<br/>
## Alternative way: Hugo
<br/>
Hugo adalah sebuah *library* untuk membuat *log executing time* pada sebuah proses menggunakan annotasi yang dibuat oleh [**Jack Wharton**](https://github.com/JakeWharton). Cara menggunakan nya sangat sederhana.
Pertama tambahkan *library* ke dalam *gradle* seperti ini :

{% highlight gradle linenos  %}
buildscript {
  repositories {
    mavenCentral()
  }

  dependencies {
    classpath 'com.jakewharton.hugo:hugo-plugin:1.2.1'
  }
}

apply plugin: 'com.android.application'
apply plugin: 'com.jakewharton.hugo'
{% endhighlight %}
<br/>

Lalu, sisipkan annotasi **@DebugLog** pada sebuah *method / function* seperti ini:
{% highlight java linenos %}
@DebugLog
public String getName(String first, String last) {
  SystemClock.sleep(15); // Don't ever really do this!
  return first + " " + last;
}
{% endhighlight %}
<br/>

Ketika dijalankan maka akan muncul *log* berisi *return data* dari *method* yang di *log* beserta waktu yang dibutuhkan untuk mengeksekusinya seperti dibawah ini :
{% highlight java %}
V/Example: ⇢ getName(first="Jake", last="Wharton")
V/Example: ⇠ getName [16ms] = "Jake Wharton"
{% endhighlight %}
<br/>

kalian juga bisa membuat *switch* untuk meng-*enable* dan men-*disable* Hugo dengan menambahkan parameter ini pada *gradle*:
{% highlight gradle linenos  %}
hugo {
  enabled false
}
{% endhighlight %}
<br/>

Lebih lanjut tentang Hugo [klik disini.](https://github.com/JakeWharton/hugo)

Jadi gitu guys cara mengukur waktu yang digunakan dalam sebuah proses, semoga membantu!
