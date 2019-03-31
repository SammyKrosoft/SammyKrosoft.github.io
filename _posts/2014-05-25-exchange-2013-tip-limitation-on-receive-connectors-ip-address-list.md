---
layout: post
title: Exchange 2013 Tip – Limitation on receive connector’s IP address list
date: 2014-05-25 20:15
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p><p>Exchange 2013 has a 1300-ish limit on the number of allowed IPs we can set in the list (the limit is on the corresponding AD attribute actually). Here are a few tips to workaround this limitation:</p><p></p><ul>   <li>create another receive connector with remaining IP addresses with the same set of permissions.</li> </ul><p>You get another 1300 ~ IPs that can be added.</p><ul>   <li>consider consolidating those IPs into supernet ranges. There are some good calculators you can use online that will do that for you. </li> </ul><p><em>For example instead of </em></p><p><em>192.168.1.1</em></p><p><em>192.168.1</em><em>.2</em></p><p><em><em>192.168.1</em>.3</em></p><p><em>Use 192.168.1.1/30</em></p><p><em>=&gt; One entry &ndash; 3 IPs</em></p><p></p><p>Thanks to <strong>Akshay Katti</strong> (PFE India) and <strong>Richard Timmering</strong> (PFE US) for these !</p><p>Cheers,</p><p>Sam.</p></p>

