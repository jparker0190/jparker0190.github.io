---
layout: post
title:      "Let's see what's moving"
date:       2018-06-09 21:56:15 +0000
permalink:  lets_see_whats_moving
---


I am a "semi" active investor and it is always helpful to know what is moving in the market and any financial website will give you that info on start up but I was more curious about how to pull that info out so I could eventually start making my own application getting the information all at once that I wanted. So i started with the below. 

First. I needed to find a website to scrape. For the purposes of this simple app I decided to use marketwatch.com because on their homepage they display S&P's biggest movers in a table in the top of the page.

In their souce code I noticed that each ticker and it's url were displayed inside an a element..
```
<a class="list__item" data-track-code="MW_Header_Movers_MNST" href="https://www.marketwatch.com/investing/stock/mnst">
                        <span class="mover__symbol">MNST</span>
```

...providing a good place for me to scrape and pull it into my app through nokogiri. 
```
doc = Nokogiri::HTML(open("https://www.marketwatch.com/"))
    stock = doc.search("a.list__item")
    stock.collect{|t| new(t.search('span.mover__symbol').text, t.attribute('href'))}
```

In my app I created a class called Stock_Find that on initialization will take two arguments ticker and url. Inside this class I have a class method called stock_scrape that will instiate the class and pass in the above code as arguments. Stock_Find also has a class method .all so I can grab the information from marketwatch and pass it to the terminal in my CLI class.

In CLI I built a method list_stocks that stores the class method StockFind.all in an instance variable @stocks...
```
@stocks = StockScraper::Stock_Find.all
```
...so that it can be iterated over and put to the console.
```
    @stocks.each.with_index(1) do |equity, i|
      puts"#{i}. #{equity.ticker}"
    end
```

After the first 8 most active stocks are displayed another method is called instructions which is used to take a user's input and display the url associated with that ticker and redirect them to marketwatch's page for that ticker...
```
input = nil
    while input != "cya"
      puts""
      puts "Enter a number for more information or type cya to leave app."
      input = gets.strip.downcase
      if input.to_i > 0
        stock_pick = @stocks[input.to_i-1]
        puts""
        puts"You can click on the link beside the ticker to be taken to the stock's website on marketwatch.com"
        puts ""
        puts"#{stock_pick.ticker} - #{stock_pick.url}"
      elsif input == "list"
        list_stocks
      elsif input == "cya"
        goodbye
      else
        puts "You didn't pick a good command. Try again."
      end
    end
```
...as you can see from above instructions will ask for a number from the ordered list that you would  like more information on.  The return value will be the ticker and the url of the ticker and since I grabbed the href in stock_scrape the user can click on the url in the terminal and be redirected to that stock's webpage. 



