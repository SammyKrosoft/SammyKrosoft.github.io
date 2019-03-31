---
layout: page
title: About
permalink: /about/
---

My name is Sam, I've been a Microsoft engineer since October 1999 in various positions such as Presales Engineer, Support Engineer, Rapid Response Engineer (RRE), Premier Field Engineer (PFE), Microsoft Service Consulting engineer (MCS), Technical Account Manager (TAM) in France and Europe, Transactinal PFE and Dedicated Engineer in Canada.
You can find more information about me on [my about.me page](https://about.me/samdrey){:target="_blank"}

I also have a PowerShell code repository on GitHub:
[SammyKrosoft's Github page][SammyKrosoft-Github]{:target="_blank"} /
[SammyKrosoft's Github page repositories](https://github.com/SammyKrosoft?tab=repositories){:target="_blank"}

And finally you can also check my [TechNet blog][SammyKrosoftTNBlog]{:target="_blank"}, which is the blog site that's being progressively replaced by this site.

Check below to see what generates this output:

```
First Name                              : Sam
Nickname                                : SammyKrosoft
Function                                : Microsoft Engineer since Oct.1999
Products Specialties                    : Windows Active Directory, Microsoft Exchange, Exchange Online (O365, MS365)
Programming Languages Learnt since 10yo : GW Basic, Turbo Pascal, C, C++, Visual Basic, Visual C#, VBScript, PowerShell

```

Starting 2019, I'll start using a new Blogging platform ... This new Blogging platform is hosted on [Github.com](http://github.com){:target="_blank"}
, and it's using the excellent [Jekyll][jekyll-gh]{:target="_blank"}


Jekyll  offers powerful support for code snippets, for exemple PowerShell below:


{% highlight powershell %}
# Starting PowerShell v3 only we can instantiate a PS Custom Object using the "adapter" [pscustomobject]
# note the [ordered] adapter just before the hash table definition, meant to keep the order in which you write the "Key = Value" pairs...
[pscustomobject][ordered]@{
  "First Name" = "Sam"
  "Nickname" = "SammyKrosoft"
  "Function" = "Microsoft Engineer since Oct.1999"
  "Products Specialties" = "Windows Active Directory, Microsoft Exchange, Exchange Online (O365, MS365)"
  "Programming Languages Learnt since 10yo" = "GW Basic, Turbo Pascal, C, C++, Visual Basic, Visual C#, VBScript, PowerShell"
}
{% endhighlight %}

Or just plain "console" text below, with the output for the above code:
```
First Name                              : Sam
Nickname                                : SammyKrosoft
Function                                : Microsoft Engineer since Oct.1999
Products Specialties                    : Windows Active Directory, Microsoft Exchange, Exchange Online (O365, MS365)
Programming Languages Learnt since 10yo : GW Basic, Turbo Pascal, C, C++, Visual Basic, Visual C#, VBScript, PowerShell
```


Check out the [Jekyll docs][jekyll-docs]{:target="_blank"} for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]{:target="_blank"}. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]{:target="_blank"}.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

[SammyKrosoftTNBlog]: http://aka.ms/sammy
[SammyKrosoft-Github]: https://github.com/SammyKrosoft
