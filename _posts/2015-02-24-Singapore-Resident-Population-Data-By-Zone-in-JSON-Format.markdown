---
layout: post
comments: true
title:  "Singapore Resident Population Data By Zone in JSON Format"
date:   2015-02-24 23:55:36
categories: Development
---

#TL;DR. 

Download json data files below 

* {% include file_link.html name="Total population in each zone" url="assets/population/total.json" %}
* {% include file_link.html name="Female population in each zone" url="assets/population/females.json" %}
* {% include file_link.html name="Male population in each zone" url="assets/population/males.json" %}

#The Story and Codes

So recently I undertook a small geospatial assignment where I needed to find out the population data of Singapore by planning zones. So I looked up in <http://data.gov.sg/> and did some searching.

What I have found is

1. There is one dataset called "Subzone Census2010" and another called "Region Census2010", but I just can't download them correctly. Every time I click on the SHP file icon, I did download a zip file, but the file is always only 22 bytes in size, so clearly there is something wrong with it and I couldn't use it directly. And you could try yourself by following links: [subzone census data](http://data.gov.sg/Metadata/OneMapMetadata.aspx?id=Subzone_Census2010&mid=163191&t=SPATIAL) and [region census data](http://data.gov.sg/Metadata/SGMatadata.aspx?id=0114350000000014649K&mid=162644&t=TEXTUAL).
2. There is no total population distribution data, but only data about resident population, which I believed consists of only Permanent Resident and Citizen based on this [report](http://www.nptd.gov.sg/portals/0/homepage/highlights/population-in-brief-2014.pdf) from National Population And Talent Division. So as an international student, I am not considered as a resident yet. :P
3. The dataset mentioned above is called [Resident Population by Planning Area/ Subzone, Age Group and Sex (2014)](http://data.gov.sg/Metadata/SGMatadata.aspx?id=0114350000000014649K&mid=162644&t=TEXTUAL), and could be downloaded from the link. I understand that this is only around 70% of the population, but this dataset is really awesome. By saying awesome I mean this dataset is very updated (lastly updated at 26-SEP-2014) and extremely detailed, which breaks population into both gender groups and age groups by very small intervals (5 years).

However, this awesome dataset is in XLS format and I couldn't do much with it directly, not mentioning using this inside my web application.

So I decided to do some clean up with the data and transformed it into something usable. And of course, using Ruby, since it's my favorite until now!

#Step 1

I exported the excel file into CSV by using excel 2010 on my mac. And I pick the 2014 CSV file since that's one relevant to me, then I name the CSV file to `exported_data_2014.csv`.

#Step 2

I did a bit search and found this gem called [smarter_csv](https://github.com/tilo/smarter_csv), and it works like a charm. So the code is 

{% highlight ruby %}
	
require 'smarter_csv'
data = SmarterCSV.process('exported_data_2014.csv')

{% endhighlight %}

#Step 3

And the above code produces results like following
{% highlight ruby %}
	
{
  "subzone": "Cheng San",
  "total": 30400,
  "0_-_4": "1,310",
  "5_-_9": "1,260",
  "10_-_14": "1,380",
  "15_-_19": "1,450",
  "20_-_24": "1,610",
  "25_-_29": "1,930",
  "30_-_34": "2,690",
  "35_-_39": "2,500",
  "40_-_44": "2,540",
  "45_-_49": "2,290",
  "50_-_54": "2,390",
  "55_-_59": "2,320",
  "60_-_64": "2,290",
  "65_&_over": "4,430"
}

{% endhighlight %}

While, this is not exactly what I want, since the key has too many dashes inside(not elegant), and the value I expect is purely number, not string with separator `,` inside.

So I did a simple looping and convert them into the format I want by following codes

{% highlight ruby %}

mappings = {
  "0_-_4": "0_4",
  "5_-_9": "5_9",
  "10_-_14": "10_14",
  "15_-_19": "15_19",
  "20_-_24": "20_24",
  "25_-_29": "25_29",
  "30_-_34": "30_34",
  "35_-_39": "35_39",
  "40_-_44": "40_44",
  "45_-_49": "45_49",
  "50_-_54": "50_54",
  "55_-_59": "55_59",
  "60_-_64": "60_65",
  "65_&_over": "65_over"
}

for d in data
  d.keys.each { |k| d[k] = d[k].to_s.gsub(/,/, '').to_i if mappings[k] }
  d.keys.each { |k| d[ mappings[k] ] = d.delete(k) if mappings[k] }
end

{% endhighlight %}

So with this snippet, I have converted the number string into actual number by removing the `,` symbol and also map the key names into the format I would like to use.

#Step 4

So now I need to slice the whole data into three sub-groups, namely "total", "female," and "male".

After examining the data for a while, I came with a simple workaround which helps me achieve the goal.

{% highlight ruby %}

total = Array.new
males = Array.new
females = Array.new

for d in data
  if d[:total] == "Males (Number)"
    sp1 = data.index(d)
    total = data.slice(1..sp1-8)
  end
  if d[:total] == "Females (Number)"
    sp2 = data.index(d)
    males = data.slice(sp1+1..sp2-8)
    ep = data.length-1
    females = data.slice(sp2+1..ep-5)
  end
end

{% endhighlight %}

Where `sp1` simply means stop point 1, `sp2` means stop point 2 and `ep` just means end point.

#Step 5

After this I realize that the `total` attribute is still in string format and I haven't converted it yet, but I couldn't directly convert it before my slicing, since I am actually using the attribute as index points.

So I did another hack on this.

{% highlight ruby %}

def convert_total(array)
  for d in array 
    d.keys.each { |k| d[k] = d[k].to_s.gsub(/,/, '').to_i if k.to_s == "total"}
  end
end

convert_total(total)
convert_total(males)
convert_total(females)

{% endhighlight %}

As simple as this, I did a hack and convert all `total` into actual numbers.


#Step 6

Now let's just write those array into files!

{% highlight ruby %}

def write_to_file(name, data)
  open(name, 'w') { |f|
    f.puts data.to_json
  }  
end

write_to_file("total.json", total)
write_to_file("males.json", males)
write_to_file("females.json", females)

{% endhighlight %}

#Step 7

All done. Sit back and have some fun with the data.