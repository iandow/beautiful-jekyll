---
layout: post
title: How To Monitor Kafka Apps With Jprofiler
tags: [jprofiler, kafka]
---

I've been spending a lot of time trying to maximize throughput for a Kafka data streaming pipeline. A large part of this effort has involved optimizations to data structures in my Java code. Generally speaking, anytime I use a data structure which is not a byte array, I sacrifice performance. But to precisely diagnose where and why my code is running inefficiently I use JProfiler. I like JProfiler because it integrates well with IntelliJ on my Mac and its user interface is nicely polished. You can learn more about JProfiler at [http://www.ej-technologies.com/products/jprofiler/overview.html](http://www.ej-technologies.com/products/jprofiler/overview.html).


Here's how I connect JProfiler on my laptop to monitor a Kafka consumer process running on my remote cluster:

First we need to install the JProfiler profiling agent on the cluster node where our application runs. Download the Jprofiler agent from [https://www.ej-technologies.com/download/jprofiler/files](https://www.ej-technologies.com/download/jprofiler/files). If you're using Linux, installation is easy. I just untar'd it to my home directory.

Back on your laptop, open JProfiler. I'm currently using JProfiler version 9.2.

<center>
<img src="http://iandow.github.io/img/JProfiler001.png" width="33%">
</center>

Next, open the Start Center.

![JProfiler002](http://iandow.github.io/img/JProfiler002.png)

Then click New Session.

![JProfiler003](http://iandow.github.io/img/JProfiler003.png)

Under the JVM Settings, specify IP address and port to the profiling agent. 

![JProfiler004](http://iandow.github.io/img/JProfiler004.png)

Once you click OK, it will wait on the following screen until you start your remote application.

<center>
<img src="http://iandow.github.io/img/JProfiler005.png" width="33%">
</center>

Finally, start your remote application with the profiling agent specified in the -agentpath command line argument, like this:

{% highlight bash %}
java -cp `mapr classpath`:nyse/nyse-taq-streaming-1.0-jar-with-dependencies.jar -agentpath:/home/mapr/jprofiler9/bin/linux-x64/libjprofilerti.so=port=11002 com.example.Run consumer /user/iandow/mystream:mytopic
{% endhighlight %}

JProfiler should automatically connect, and you should be able to see really useful information about how your code is running, like this:

![JProfiler006](http://iandow.github.io/img/JProfiler006.png)

All done!

<br><br>
<div class="main-explain-area padding-override jumbotron">
  <a href="https://www.paypal.me/iandownard" title="PayPal donation" target="_blank">
  <h1>Hope that Helped!</h1>
  <img src="http://iandow.github.io/img/starbucks_coffee_cup.png" width="120" style="margin-left: 15px" align="right">
  <p class="margin-override font-override">
    If this post helped you out, please consider fueling future posts by buying me a cup of coffee!</p>
  </a>
  <br>
</div>