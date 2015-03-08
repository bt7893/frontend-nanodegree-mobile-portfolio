## Website Performance Optimization Pizza project:

### Instructions:

1. Launch the URL : bt7893.github.com/frontend-nanodegree-mobile-portfolio/views/pizza.html
2. Move the slider to change the pizza size (open web inspector console log to check the time it takes to redraw the page)
3. Scrolling the page - the console log should show 60 frames under 1ms (which means each redraw is higher than 60fps)

### Optimizations Performed

1. Inline CSS styling:
    > pizza.html - embedded the style.css file
2. Optimized images:
    > pizza.png - reduced size, removed metadata and saved as PNG 8 bit
    > pizzeria.jpg - reduced size, removed metadata and saved at 100px x 73px
3. main.js file:
    > document.querySelectorAll(".randomPizzaContainer") variable - moved this variable so that it sits outside the function to reduce iterations to fetch the data.
4. Minify js
    > main.js --> http://jscompress.com/
5. Minify css
    > bootstrap-grid.css --> http://cssminifier.com/
