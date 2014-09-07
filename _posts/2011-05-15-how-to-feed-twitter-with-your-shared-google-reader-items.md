---
title: How To Feed Twitter with Your Shared Google Reader Items
author: Ken Haigh
layout: post
permalink: /posts/how-to-feed-twitter-with-your-shared-google-reader-items/
dsq_thread_id:
  - 304820373
categories:
  - Social Media
  - Technology
tags:
  - Google
  - googlereader
  - 'HootSuite - Social Media Dashboard'
  - News aggregator
  - RSS
  - Twitter
  - TwitterFeed
---
I am an avid <a title="Google Reader" href="http://reader.google.com" target="_blank">Google Reader</a> user. Even though the popularity of RSS readers has waned over the last year, Google Reader is still the fastest interface to consume information about the topics that interest me. How can you go wrong with a service where j/k keys are down/up?  
<img class="alignleft size-full wp-image-113" title="Feed Google Reader To Twitter" src="/wp-content/uploads/2011/05/googlereadertweet.png" alt="" width="196" height="196" />One problem I had with Google Reader is that I wanted to easily tweet my shared items to my friends on Twitter. I found a number of solutions with RSS feed functionality such as Hootsuite and TwitterFeed, but was not happy with any of them. Some items were not tweeted and I wanted more flexibility in the scheduling of tweets and formatting. I also found services like Posterous that are integrated with Google Reader to be cumbersome. I want just want to click “shift-s” to share and move on.

After investigating the problem with inconsistent feeding, I believe I understand the issue.

<!--more-->

The Google Reader shared items are aggregated from a number of different RSS feeds. As a result, the published dates for the entries are not consistent.  Your Google Reader shared items do not behave as a typical RSS feed where each item has a newer published date. The generic RSS feed functionality in a service such as Hootsuite uses the published date to determine the next item to pull from the feed 

**instead of the time when you shared the item**. As a result, items that you shared will get skipped if the next item was published before the item that was just tweeted.

This problem can be solved. I created a simple bash shell script that will tweet your Google Reader shared items to Twitter by using the timestamp when the item was shared instead of the published date.

**How to Install on MAC OS X**

1.  **Install xmlstartlet**. The script has one dependency called xmlstarlet. Xmlstarlet is one of my favorite command line utilities useful for formatting xml and parsing xml files using xslt type processing. Visit <a title="Building Xmlstarlet in Snow Leopard OSX" href="http://blog.troyastle.com/2010/03/building-xmlstarlet-in-snow-leopard-osx.html" target="_blank">this site</a> for directions on how to install.
2.  **Download the script**. I uploaded the script to my github repository. <a title="Download page for shgr.sh" href="https://github.com/khaigh/Social-Media-Scripts" target="_blank">Download shgr.sh</a> from github and copy the downloaded file into your user directory.  You will need to give execute permission to the script by running: 
    <pre>chmod 711 shgr.sh</pre>

3.  **Configure the script**. You will need to open shgr.sh in your favorite text editor and replace the configuration variables TWITTER\_USERNAME and TWITTER\_PASSWORD with your twitter username and password. 
    <pre>TWITTER_USERNAME="&lt;insertusername&gt;"
TWITTER_PASSWORD="&lt;insertpassword&gt;"</pre>

4.  **Find your shared items URL**.   Login into Google Reader and find your feed URL.  Copy your feed URL to the clipboard. <div style="display: block; height: 135px;">
      <img class="alignleft size-full wp-image-80" title="Feed URL for Google Reader Shared Items" src="/wp-content/uploads/2011/05/googlereader2.png" alt="" width="530" height="116" />
    </div>

5.  **Schedule the script as a cron job**. Cron is a time based schedule for unix-like computers such as MAC OS X. [Follow the instructions here][1] on installing a new crontab. The row in your crontab file should look similar to this: 
    <pre>*/30 * * * * /Users/ken/shgr.sh http://www.google.com/reader...</pre>
    
    The &#8217;30&#8242; represents the number of minutes between tweets when you have multiple items in your shared items.</li> </ol> 

**Important Notes**

*   Crontab does not handle ‘%’ without escaping.  Please escape the ‘%’ by placing a ‘\’ in front of each ‘%’ in your shared URL.
*   Cron only runs when your computer is on.  If you shut down your computer, the cron job will not tweet items from your Google Reader shared items until you turn on the computer.
*   The script will initially use the latest 20 Google Reader shared items.  If you don’t want all 20 items to be tweeted,  you may remove older entries by editing the temporary file (default:  /tmp/shgr.txt) after the first time the script is run by cron.
*   The script stores the twitter password in cleartext in the script.   This is insecure and a bad practice.  If you share your machine with others, I would recommend storing the password in the keychain.

**What Does the Script Do?**

The script retrieves the Google Reader RSS shared item feed and filters for new items using the Google timestamp.   The new items are placed in a temporary file.  The script then grabs the oldest item in the temporary file, shortens the URL, tweets the title with the shortened URL, and removes the item upon success.

Possible enhancements include adding sharing to Facebook, using the keychain for passwords, and scheduling your own tweets.

If you have any trouble installing the script or making it run, please let me know.  I would be happy to help you.

**Update:** I added the capability to use create hashtags for the tweet based on the categories of a given post. This change has been checked in to the git repo.

<!-- Start Shareaholic Recommendations Automatic -->

<!-- End Shareaholic Recommendations Automatic -->

[1]: http://benr75.com/pages/using_crontab_mac_os_x_unix_linux "Using Crontab on MAC OS X"
