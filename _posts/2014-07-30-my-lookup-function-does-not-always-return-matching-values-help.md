---
layout: post
title: My LOOKUP function does not always return matching values, help !!
date: 2014-07-30 14:25
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;</p><p>Lost of times I work with Excel to make statistics, build nice operational Exchange dashboards, and I make a very intensive usage of LOOKUP and VLOOKUP. Since I recently lost valuable time trying to figure why a LOOKUP would not return me the values I want, or sometimes it did, sometimes it didn’t, I finally found the workaround for this.</p><p>This post is mostly for me to remember the trick but it’s good to know for you guys as well if you are like me an Excel-man (ish):</p><p>&nbsp;</p><p><font size="3">This post is merely a pure copy (commented) from the following article:</font></p><p><a title="http://support.microsoft.com/kb/181212" href="http://support.microsoft.com/kb/181212"><font size="3">http://support.microsoft.com/kb/181212</font></a></p><p>&nbsp;</p><p><strong>“LOOKUP requires that the first column of the vector (or the first column or row for the array form) is sorted in ascending order. The following information describes different formulas that you can use to return the same information returned by LOOKUP without requiring that the first column of the table be sorted. “</strong></p><p>==&gt; because I was not aware of the above until I searched the Internet (using <a href="http://www.bing.com">www.bing.com</a> I have to say it), I lost about 2 hours trying to figure what was wrong in my spreadsheet ! I was thinking “Chier, qu’est-ce que c’est encore que ce truc de daube ???” which the Parisian French for “what is wrong with my data ???” </p><p>(Private message to my friends Joe T. and Liju V.: we don’t say “sacrebleu” anymore in France since the XVIIth century, you have to learn the expression above instead :-) )</p><p>&nbsp;</p><p>Then I found the above article, which saved my spreadsheet. Pewwww, it’s not a bug, it’s just the way the LOOKUP function works !! I was just ignorant.</p><p>I could have used VLOOKUP, but VLOOKUP does a lookup on the first column only. so if you wish to match the data on something in a column of your table that is not in the first column, and you don’t want to rearrange your original table against which you are searching, that’s when you’d use the formula in mentionned this article.</p><p>I found the “INDEX+MATCH” combination the most useful and the most practical to use in my case, so I paste this part in this post. For the other possibilities, just open the support article above (181212)</p><p>&nbsp;</p><p>&nbsp;</p><h5><em><font size="3">Using INDEX and MATCH</font></em></h5><em><font size="3">The following formula returns the same information that a LOOKUP returns without requiring the first column of the table to be sorted: 
</font></em><p><code></code></p><code><pre><em><font size="3">   =INDEX(Table_Array,MATCH(Lookup_Value,Lookup_Array,0),Col_Index_Num)
				</font></em></pre></code><p><em><font size="3">Where: 
</font></em><pre><em><font size="3">   Table_Array    = The entire lookup table.

   Lookup_Value   = The value to be found in the first column of
                    &quot;table_array&quot;.

   Lookup_Array   = The range of cells containing possible
                    lookup values.

   Col_Index_Num  = The column number in &quot;table_array&quot; for which
                    the matching value should be returned.
</font></em>				</pre><code><pre>&nbsp;</pre><pre>
</pre></code></p>
