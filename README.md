## Website Performance Optimization:

### Instructions:

1. Launch the URL : bt7893.github.com/frontend-nanodegree-mobile-portfolio/index.html

##### CRP for index.html

1. Inline CSS styling:
    > index.html - embedded the style.css file
    > added media="print" for print.css since it is render blocking
2. Optimized images:
    > profilepic.jpg - reduced size, removed metadata
3. Added async attribute for perfmatters.js:
    > added the async attribute so that the script is executed asynchronously as soon as it is available.
4. Minify js
    > perfmatters.js --> http://jscompress.com/ 
5. Added .htaccess file to leverage browser caching 

# Results 
1. PageSpeed Insights: 
    > Mobile: 95/100
    > Desktop: 96/100
    
      
##### Cam's Pizzeria

1. Launch the URL : bt7893.github.com/frontend-nanodegree-mobile-portfolio/views/pizza.html
2. Move the slider to change the pizza size (open web inspector console log to check the time it takes to redraw the page)
3. Scrolling the page - the console log should show 60 frames under 1ms (which means each redraw is higher than 60fps)

# Optimizations Performed

1. Inline CSS styling:
    > pizza.html - embedded the style.css file
2. Optimized images:
    > pizza.png - reduced size, removed metadata and saved as PNG 8 bit
    > pizzeria.jpg - reduced size, removed metadata and saved at 100px x 73px
3. main.js file:
    > document.querySelectorAll(".randomPizzaContainer") variable - moved this variable so that it sits outside the function to reduce iterations to fetch the data.
    > reduced number of sliding pizzas generated from 200 to 30. You only need 30 to fill the page.
4. Minify js
    > main.js --> http://jscompress.com/
5. Minify css
    > bootstrap-grid.css --> http://cssminifier.com/
    
6. As a result of minify js, comments were stripped, thus I am including the original code below:
# Original 
      // Iterates through pizza elements on the page and changes their widths
      function changePizzaSizes(size) {
        for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
          var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
          var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
          document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
        }
      }
# Modified
      var randomPizza = document.querySelectorAll(".randomPizzaContainer"); // moved this variable so that it sits outside the function to reduce iterations to fetch the data.
      // Iterates through pizza elements on the page and changes their widths
      function changePizzaSizes(size) {
        for (var i = 0; i < randomPizza.length; i++) {
          var dx = determineDx(randomPizza[i], size);
          var newwidth = (randomPizza[i].offsetWidth + dx) + 'px';
          randomPizza[i].style.width = newwidth;
        }
      }
# Original
      // Generates the sliding pizzas when the page loads.
      document.addEventListener('DOMContentLoaded', function() {
        var cols = 8;
        var s = 256;
        for (var i = 0; i < 200; i++) {
          var elem = document.createElement('img');
          elem.className = 'mover';
          elem.src = "images/pizza.png";
          elem.style.height = "100px";
          elem.style.width = "73px";
          elem.basicLeft = (i % cols) * s;
          elem.style.top = (Math.floor(i / cols) * s) + 'px';
          document.querySelector("#movingPizzas1").appendChild(elem);
        }
        updatePositions();
      });
      
# Modified
// Generates the sliding pizzas when the page loads.
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 30; i++) {  // changed from 300 to 30 pizzas
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});


# Results
1. Page Speed over 60fps during scrolling (via console.log)
2. Time to resize under 5ms


