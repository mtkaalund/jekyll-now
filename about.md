---
layout: page
title: About
permalink: /about/
---

{% avatar mtkaalund %}


Nothing here yet :)

TEST TEST

{% highlight cpp %}
#include <vector>
#include <iostream>

int main( void )
{
  std::vector<int> vecInts;

  vecInts.push_back(2);
  vecInts.push_back(8);

  for( auto item : vecInts )
  {
    std::cout << "Item value: " << item << std::endl;
  }	

  return 0;
}
{% endhighlight %}
