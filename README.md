# Website Performance Optimization Portfolio Project
This is the first project of the Advanced Interactive Websites chapter of Udacity's Front-End Web Developer nanodegree. This project aims to give the studnets a great push into the world of performance, because #perfmatters .

### Part1: Optimize PageSpeed Insights score for index.html

I will explain what I did in steps for how I got 90+ score.
1. Test the page using [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/).
2. Download optimized resources including images, css, javascript provided from PageSpeed Insights.
3. Following PageSpeed Insights recomandations, and these are:
a. Replaced css, javascript, images with optimized files.
b. Moving javascript to bottom of `<body>`.
c. Inline Javascript.
d. Using `async` with Javascript to unblock html parsing.
e. Using `media="print"` with the print css (/css/print.css).
f. [Inline](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery) style.css.

Until now I was using [python http server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/set_up_a_local_testing_server) or [Fenix](http://fenixwebserver.com/) to run a local server, and test the page using a link by [ngrok](https://ngrok.com/) in PageSpeed Insights. After all the steps above I was getting 92 score on desktop and 70-80 on mobile, and PageSpeed Insights recomanded that I reduce the server response time, after a long search I uploaded the page to a server that already has files compression on it, and I got amazing results.

[**Check a live version here**](http://mrosamah.com/perfmatters/).

![mobile-test](https://mrosamah.github.io/frontend-nanodegree-mobile-portfolio/score-mobile.jpg)
![desktop-test](https://mrosamah.github.io/frontend-nanodegree-mobile-portfolio/score-desktop.jpg)

### Part2: Optimize Frames per Second in pizza.html
In this part there was two tasks to do, one with the pizzas in the background, and the other was with the pizza size changing slider (both are Forced Synchronous Layout problems).

I solved the first one by moving what triggres the layout in the loop inside `updatePositions()` function outside.

```
function updatePositions() {
  // ..
  // ..
  var items = document.querySelectorAll('.mover');
  // Moving what triggers layout outside the loop
  var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
  // ..
  // ..
}
```

The second one was almost the same, but the function `changePizzaSizes(size)` was doing comlicated work, so instead this is a less complicated and straigt forwoard one:

```
  function changePizzaSizes(size) {
    // gatherning all the selector in one varible
    var randomPizzaContainer = document.querySelectorAll(".randomPizzaContainer");
    // used to store the new width percentages
    var newWidth;
    // Based on slider change newWidth
    switch (size) {
      case "1":
        newWidth = 25;
        break;
      case "2":
        newWidth = 33.3;
        break;
      case "3":
        newWidth = 50;
        break;
      default:
        console.log("Yo");
    }
    // Iterates through pizza elements on the page and changes their widths
    for (var i = 0; i < randomPizzaContainer.length; i++) {
      randomPizzaContainer[i].style.width = newWidth + "%";
    }
  }
```