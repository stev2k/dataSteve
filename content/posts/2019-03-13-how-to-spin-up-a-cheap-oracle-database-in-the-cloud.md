---
title: How to spin up a cheap Oracle Database in the cloud
author: Steve Haines
date: 2019-03-13T22:40:13+00:00
weight: 1
featured_image: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/oracle-1194372_1920-1568x1049.jpg
categories:
  - HowTo
tags:
  - Cloud
  - Database
  - Dev/Test
  - Environment
  - Oracle
  - PaaS
  - Sandbox

---
I saw [this article][1] from linked in a tweet yesterday and realised I know very little about Oracle&#8217;s cloud offering. It seems like this venerable giant of the data industry is not leading the way in the cloud domain. As a curious type, I thought it would be interesting to investigate how to spin up a cheap Oracle database in the cloud using Oracle&#8217;s own little known cloud offering and others.

Obviously the article doesn&#8217;t make it sound great but it can&#8217;t be all that bad right?

### Or maybe it is that bad?

I wasn&#8217;t initially even sure if it was possible to run an Oracle database without paying the hefty licence fees. After a little exploration it does seem like you have a few options:

  * AWS (DBaaS and IaaS)
  * Oracle Cloud (DBaaS and IaaS)
  * Other Vendors including Google Cloud Platform and Azure (IaaS only)
  * The only vendors that offer Oracle Database as a Service are Amazon and Oracle it seems. Every other vendor would provide only bare metal which is not what i need for this experiment. Managing the infrastructure and software config may well be your jam and if that&#8217;s the case, power to you! For mission critical applications it can certainly be an advantageous thing as your get powerful hardware dedicated to your application.

### Optimising for cheapness&#8230;

I thought I&#8217;d give Oracle some credit and see what they are offering for the casual user. However, I was immediately put off when I saw a few of their &#8216;cost calculator&#8217; pages. The first [Oracle Cloud pricing calculator][2] I found offered me a single oracle instance for over £400 a month. 

<div class="wp-block-image">
  <figure class="aligncenter"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11-1024x500.png" alt="Oracle Cloud DBaaS (PaaS) monthly prices for Oracle database" class="wp-image-42" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11-1024x500.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11-300x147.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11-768x375.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-11.png 1535w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></figure>
</div>

I&#8217;m expecting that maybe when you compare the costs with the performance that you are getting for that price, it may be competitive than it seems. However for my experiment I was really looking for something which is sub-£100 a month. If you&#8217;d like to compare the costs of Oracle Cloud with AWS, below is the hourly cost of Oracle&#8217;s offer:

<div class="wp-block-image">
  <figure class="aligncenter"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3-1024x263.png" alt="The cost of running an Oracle database in the cloud on Oracle's own cloud platform." class="wp-image-27" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3-1024x263.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3-300x77.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3-768x197.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-3.png 1504w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></figure>
</div>

After some searching I found the equivalent [pricing calculator for AWS][3] and found a very different story. Amazon offers two licencing options for Oracle on RDS:

  1. Pay as you go
  2. Bring your own licence.

The Amazon calculator gave me a much more friendly result for my cheap Dev/Test Oracle environment:

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5-1024x823.png" alt="Amazon RDS pricing for Oracle Datbase using AWS and t3.Micro instance" class="wp-image-30" width="1024" height="823" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5-1024x823.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5-300x241.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5-768x617.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-5.png 1479w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></figure>
</div>

The monthly estimated cost works out as roughly $30 a month which seems totally reasonable given the tiny compute instance I have requested (db.t3.micro)

### Setting up an Oracle database in RDS

If you&#8217;d like to do this, it&#8217;s easy&#8230;.assuming you already have an AWS account, just select RDS in the products drop down list and click on Create Database as below.

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4-1024x254.png" alt="Amazon RDS cloud relational database in the cloud - a PaaS relational database service that can scale." class="wp-image-28" width="1024" height="254" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4-1024x254.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4-300x74.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4-768x190.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-4-1568x388.png 1568w" sizes="(max-width: 1024px) 100vw, 1024px" /></a></figure>
</div>

Next, I select Oracle from the database engine options page. The free tier is actually available for use with Oracle, but you must already have a licence to use the software.

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6-1024x1005.png" alt="Amazon RDS engine options, including Aurora, MySQL, MariaDB, PostgreSQL, Oracle and Microsoft SQL Server." class="wp-image-33" width="768" height="754" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6-1024x1005.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6-300x294.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6-768x754.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-6.png 1234w" sizes="(max-width: 768px) 100vw, 768px" /></a></figure>
</div>

I selected the Oracle Standard Edition One as it&#8217;s the cheapest but your demands might be different. I also selected a Dev/Test instance on the following page as seen below for the same reason.

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7-1024x547.png" alt="Choose between Dev/Test Oracle environment or Production environment on Amazon RDS." class="wp-image-34" width="768" height="410" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7-1024x547.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7-300x160.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7-768x410.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-7.png 1098w" sizes="(max-width: 768px) 100vw, 768px" /></a></figure>
</div>

This is the final page of config, and where we decide what our licencing model should be (BYOL or Licence Included in monthly cost) as well as choosing the version of the Oracle engine we would like to run, and compute & storage options for the RDS instance we are building.

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8-771x1024.png" alt="Amazon RDS Oracle license included or BYOL (bring your own licence)" class="wp-image-35" width="578" height="768" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8-771x1024.png 771w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8-226x300.png 226w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8-768x1021.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-8.png 918w" sizes="(max-width: 578px) 100vw, 578px" /></a></figure>
</div>

Finally we get an estimate of monthly running costs and we get to set up an SA user, note this info down somewhere as it&#8217;s important for obvious reasons.

<div class="wp-block-image">
  <figure class="aligncenter"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-9.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-9.png" alt="Estimated monthly cost of Oracle on Amazon RDS." class="wp-image-36" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-9.png 968w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-9-300x277.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-9-768x709.png 768w" sizes="(max-width: 968px) 100vw, 968px" /></a></figure>
</div>

### Logging in for the first time

Once you have clicked through a couple more OKs you are all set! My instance took about 5 mins to bring online which is amazing when you think how long that would take by literally any other means. Opening up Oracle SQL Developer ([download link][4]) on my laptop took almost as long.

After struggling to connect initially, I found that you need to enter the SID for Amazon RDS as &#8216;ORCL&#8217;. This is customisable, but the default for RDS Oracle instances is ORCL.

<div class="wp-block-image">
  <figure class="aligncenter"><a href="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-10.png"><img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-10.png" alt="Connecting to Oracle hosted on Amazon RDS using Oracle SQL Developer" class="wp-image-38" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-10.png 758w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/image-10-300x158.png 300w" sizes="(max-width: 758px) 100vw, 758px" /></a></figure>
</div>

That&#8217;s actually it&#8230;this whole process took around 30 mins or less, pretty impressive. I actually found this much easier than I thought it would be. [Amazon RDS][5] really is an incredible product and would recommend to anyone who looking to spin up an quick database test environment. I can&#8217;t vouch for the performance as I haven&#8217;t done any performance testing on this instance, but as far as price is concerned it&#8217;s a winner! I hope this blog helped you to understand how to spin up a cheap oracle database in the cloud without lightening your wallet significantly by using Oracle&#8217;s own services.

[1]: https://t.co/YW3ZMOpccs
[2]: https://cloud.oracle.com/en_US/cost-estimator
[3]: https://aws.amazon.com/rds/pricing/
[4]: https://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html
[5]: https://aws.amazon.com/rds/