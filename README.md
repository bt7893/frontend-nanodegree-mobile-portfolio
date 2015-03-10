# Website Performance Optimization:

## CRP for index.html

1. Launch the URL : [Frontend Nano-degree Mobile Portfolio](bt7893.github.io/frontend-nanodegree-mobile-portfolio/index.html)

### Optimizations Performed

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

#### Results
1. PageSpeed Insights:
    > Mobile: 95/100
    > Desktop: 96/100


***


## Cam's Pizzeria
1. Launch the URL : [Cam's Pizzeria](bt7893.github.com/frontend-nanodegree-mobile-portfolio/views/pizza.html)
2. Move the slider to change the pizza size (open web inspector console log to check the time it takes to redraw the page)
3. Scrolling the page - the console log should show 60 frames under 1ms (which means each redraw is higher than 60fps)

### Optimizations Performed
1. Inline CSS styling:
   - pizza.html - embedded the style.css file
2. Optimized images:
   - pizza.png - reduced size and removed metadata
   - pizzeria.jpg - reduced size and removed metadata
3. modified CSS .mover where "width: 100px" is added to eliminate the process of resizing during the iterative   
   function
4. main.js file
    > Please see detailed notes below
5. Minify js
    > main.js using [Minify JS](http://jscompress.com/)
6. Minify css
    > bootstrap-grid.css using [CSS Minifier](http://cssminifier.com/)

#### Changing Pizza Sizes
1. Modified the for loop to be more efficient
2. Reduced number of sliding pizzas generated from 200 to 30. You only need 30 to populate the page.
3. Removed the width and height style to eliminate the process of resizing. Pizza.png were resized in Photoshop as 
   100px x 77px. Metadata is removed to reduce filesize.

##### Original:
      // Iterates through pizza elements on the page and changes their widths
      function changePizzaSizes(size) {
        for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
          var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
          var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
          document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
        }
      }
##### Modified:
      var randomPizza = document.querySelectorAll(".randomPizzaContainer"); // moved this variable so that it sits outside the function to reduce iterations to fetch the data.
      // Iterates through pizza elements on the page and changes their widths
      function changePizzaSizes(size) {
        var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
        var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
        var pizzaCount = document.querySelectorAll(".randomPizzaContainer");
        for (var i = pizzaCount.length; i--;) {
          pizzaCount[i].style.width = newwidth;
        }
      }

#### Sliding Pizza Generator
1. Modified the for loop to be more efficient
2. Reduced number of sliding pizzas generated from 200 to 30. You only need 30 to populate the page.
3. Removed the width and height style to eliminate the process of resizing. Pizza.png were resized in Photoshop as 
   100px x 77px. Metadata is removed to reduce filesize.

##### Original:
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

##### Modified:
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
for (var cols = 8, s = 256, i = 30; i--; ) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    movingPizzas1tag.appendChild(elem);
  }
  updatePositions();
});

#### updatePositions
Added cachedScrollTop and placed it outside the for loop

##### Original:
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }

##### Modified:
// Moves the sliding background pizzas based on scroll position
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");
  var items = document.querySelectorAll('.mover');
  var cachedScrollTop = document.body.scrollTop;
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((cachedScrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }

### Results
1. Page Speed over 60fps during scrolling (via console.log)
2. Time to resize under 5ms

### References
1. https://css-tricks.com/authoring-critical-fold-css/
2. https://www.igvita.com/slides/2012/devtools-tips-and-tricks/jank-demo.html

### Tools
1. Adobe Photoshop CS6 - https://www.adobe.com/products/photoshop.html?promoid=KLXLS
2. ImageMagick - http://www.imagemagick.org/
3. Minify JS - http://jscompress.com
4. Minify CSS - http://cssminifier.com
