
### cheerio
Fast, flexible & lean implementation of core jQuery designed specifically for the server.
```
# https://github.com/cheeriojs/cheerio#sponsors
const cheerio = require('cheerio')
const $ = cheerio.load('<h2 class="title">Hello world</h2>')

$('h2.title').text('Hello there!')
$('h2').addClass('welcome')

$.html()
//=> <h2 class="title welcome">Hello there!</h2>
```

### SuperAgent
superagent它是一个强大并且可读性很好的轻量级ajaxAPI，是一个关于HTTP方面的一个库，而且它可以将链式写法玩的出神入化。
```
# https://github.com/wangning0/Autumn_Ning_Blog/blob/master/blogs/3-19/superAgent_learn.md
# https://github.com/visionmedia/superagent
```
