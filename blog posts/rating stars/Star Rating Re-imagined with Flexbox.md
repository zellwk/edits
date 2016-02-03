# Star Rating Re-imagined with Flexbox

The rating element is one UI pattern that you'll find everywhere on the web. I found myself building another rating element for the second time in my life a few weeks ago.

Since it's my second time working on the same element, I wanted to challenge myself to come up with a way that uses as little code as possible. Consequently, I managed to come up with a way to build the rating element with only 50 lines of code (HTML, CSS and JavaScript combined), which is incredibly sweet! This article shows how I did it.

## Demo

Before we begin, let's first take a look at the final piece of work we're building in this article:

<p data-height="266" data-theme-id="7929" data-slug-hash="bEYRbV" data-default-tab="result" data-user="zellwk" class='codepen'>See the Pen <a href='http://codepen.io/zellwk/pen/bEYRbV/'>Ratings with Flex</a> by Zell Liew (<a href='http://codepen.io/zellwk'>@zellwk</a>) on <a href='http://codepen.io'>CodePen</a>.</p>

Ready to move on? Let's begin by talking about the markup used in this rating element.

## The Markup

I wanted the rating element to use as little code as possible. Naturally, this meant the markup should be simple. The ideal rating element would contain a markup similar to the following:

```html
<div class="stars">
  <div class="star"></div>
  <div class="star"></div>
  <div class="star"></div>
  <div class="star"></div>
  <div class="star"></div>
</div>
```

Next, I also wanted to be able to use SVGs to create stars because they're scalable and flexible. While doing so, I decided to use a combination of the `<symbol>` and `<use>` elements to create the stars. I'm not going to cover what `<symbol>` and `<use>` are in this article since it's out of scope. If you want to find out more, do check out [this article by](https://css-tricks.com/svg-symbol-good-choice-icons/) Chris Coyier.

Here's the SVG I used for the star. You can build this easily with any vector tool like (Illustrator or Sketch):

```html
<svg style="display: none;">
  <symbol id="star" viewBox="0 0 98 92">
  <title>star</title>
  <path stroke='#000' stroke-width='5' d='M49 73.5L22.55 87.406l5.05-29.453-21.398-20.86 29.573-4.296L49 6l13.225 26.797 29.573 4.297-21.4 20.86 5.052 29.452z' fill-rule='evenodd'/>
</svg>
```

My markup for the rating element effectively became the following because of the SVGs:

```html
<div class="stars">
  <a class="star"><svg><use xlink:href="star"></svg></a>
  <a class="star"><svg><use xlink:href="star"></svg></a>
  <a class="star"><svg><use xlink:href="star"></svg></a>
  <a class="star"><svg><use xlink:href="star"></svg></a>
  <a class="star"><svg><use xlink:href="star"></svg></a>
</div>
```

That's all the markup I used in the rating element.

I also had to change the sizes of the stars and ensure they were white at this point:

```css
svg {
  width: 2em;
  height: 2em;
  fill: white;
}
```

![](stars.png)

The next challenge at this point was to make sure the correct number of stars got filled up whenever a star was hovered on.

## Filling up stars

Filling up an individual star is easy. All you have to do is to change the SVG's fill property whenever the star is hovered in:

```css
.star:hover svg {
  fill: orange;
}
```

What we want to do is to make sure all the stars to the left of the hovered stars get filled up as well. At first glance, there's no way to do this without using some JavaScript trickery.

Luckily, there's a way to overcome this problem without using JavaScript. The trick is to use a combination of the general sibling selector (`~`) and the flexbox `flex-flow` property.

Let's first take a look at what `~` does.

The `~` selector helps to select all sibling elements after itself. It can help us select all stars to the right of the hovered star:

```css
.star:hover svg,
.star:hover ~ .star svg {
  fill: orange;
}
```

![](gif-right-stars.gif)

Knowing this, if we can reverse the elements such that the first star is the rightmost star, and the fifth star is the leftmost star, we can use `~` to fill up the correct number of stars.

Turns out, we can reverse elements with Flexbox's `row-reverse` value.

```css
.stars {
  display: flex;
  flex-flow: row-reverse;
}
```

Try inspecting the stars once you've attached the `row-reverse` property to `.stars`. You'll see something like the following:

![](row-reverse.gif)

Notice how the first star in the DOM is now the rightmost star while the last star in the DOM is the leftmost star?

Now, if you hover over the second star from the left, you'll see that two stars are filled up. That's exactly what we need!

![](hover-left.gif)

Let's dive deeper and see why this works.

If you're hovering over the second star from the left, you're effectively hovering onto the fourth star in the DOM. The `.star:hover svg` selector we used matches the fourth star and changes it's fill to `orange`.

The `.star:hover ~ .star svg` selector matches any stars that come after the hovered star. In this case, that's the fifth star (or the leftmost star). Hence, the leftmost star gets filled up with `orange` as well.

Yay! Problem solved with only three lines of CSS! :)

Let's move on to the next challenge. If the user hovers over the second star on the left, we (visually) know that two stars are filled up. Now, all we need is to know how many stars are filled up programmatically so we can do something with it.  

## Detecting the number of stars

With our current markup and CSS, the only way to detect the number of stars is to use JavaScript. Make sure you disable `pointer-events` on the SVG elements before we move on since it'll mess around with the JavaScript functionality.

```css
svg {
  pointer-events: none;
}
```

We're ready to move on once you have pointer events turned off.

The general logic for getting the correct number of stars is this:

1. Find the total number of stars
2. Find out which star is being clicked on.
3. Calculate the correct number of stars filled.

Finding the total number of stars is quite straightforward. We just need to get the total number of children elements within `.stars`. Here's the code if you used jQuery:

```javascript
var totalStars = $('.stars').children().length
console.log(totalStars) // => 5
```

Although jQuery is helpful, I encourage you to switch to using vanilla JavaScript since there's nothing we can't do without jQuery anymore. The code for the first part is just slightly more complicated. Here's the implementation without using jQuery:

```javascript
var starContainer = document.querySelector('.stars')
var starsNodeList = starContainer.children
var stars = Array.prototype.slice.call(starsNodeList);
var totalStars = stars.length

console.log(totalStars) // => 5
```

Here, we first have to grab the children elements within the `.stars` container with the `.children` method. This method returns a HTML Node List.

We can't do anything with a HTML Node List, so we have to convert it into an array with `Array.prototype.slice.call()`.

Once it's converted into an array, we can use the `array.length` method to get the total number of stars.

A slightly condensed version of the above code is:

```javascript
var starContainer = document.querySelector('.stars')
var stars = Array.prototype.slice.call(starContainer.children);
var totalStars = stars.length

console.log(totalStars) // => 5
```

Next, we need to find out which star is being clicked on. Here's the code if you used jQuery:

```javascript
$('.stars').on('click', function(e) {
  var index = $(e.target).index()
})
```

And the code if you used vanilla JavaScript:

```javascript
starContainer.addEventListener('click', function(e) {
  var index = stars.indexOf(e.target)
})
```

If you did a `console.log()` of the index, you'll see that the index is

- 4 if you clicked on the leftmost star.
- 3 if you clicked on the second star.
- 2 if you clicked on the third star.
- 1 if you clicked on the fourth star.
- 0 if you clicked on the fifth star.

![](starsindex.gif)

The number of stars filled is hence, the total number of stars minus the index. With jQuery, the code is:

```javascript
$('.stars').on('click', function(e) {
  var index = $(e.target).index()
  var count = totalStars - index

  // Do something with the count
})
```

And with vanilla JavaScript, the code is:

```javascript
starContainer.addEventListener('click', function(e) {
  var index = stars.indexOf(e.target)
  var count = totalStars - index

  // Do something with the count
})
```

Whoohoo!

Here's the problem: you'll notice the count is 6 if you clicked on the right of the stars

![](starsindexoutside.gif)

This is because the flexed element spans across the entire width, so we're still clicking on it. One way to resolve this is to add a `display: inline-block` wrapper around the `.stars` container. Another way is to make sure you listen to click events on the individual stars instead of the `.stars` container. Either way is fine. I personally prefer the first method since it's slightly more performant.

There's only one more thing to do: ensure the rating state is saved once the user clicks on it.

## Saving the rating state

We can save the rating element's state easily by adding a class like `is-selected` whenever the user clicks on a star.

Here's how you do it with jQuery:

```javascript
$('.stars').on('click', function(e) {
  // ... get count and do something with count  

  $(e.target).addClass('is-selected')
})
```

And how you do it with vanilla JavaScript:

```javascript
starContainer.addEventListener('click', function(e) {
  // ... get count and do something with count  

  e.target.classList.add('is-selected')
})
```

Then, in your CSS, you can use the same `~` trick to style the filled stars. Make sure you place this above the `.star:hover` selectors so the lighter orange color kicks in whenever the user hovers over the stars again.

```css
.star.is-selected svg,
.star.is-selected ~ .star svg {
  fill: #996300;
}
```

If the user clicks on a new star rating, we want to update the new state. This means we should also remove all `is-selected` classes from all `.star` elements before adding the new `is-selected` class to the star that was clicked on.

Here's how you do it in jQuery:

```javascript
$('.stars').on('click', function(e) {
  // ... get count and do something with count  
  $(e.target).siblings().removeClass('is-selected')
  $(e.target).addClass('is-selected')
})
```

And vanilla JavaScript:

```javascript
starContainer.addEventListener('click', function(e) {
  // ... get count and do something with count  

  stars.forEach(function(el) {
    el.classList.remove('is-selected')
  })
  e.target.classList.add('is-selected')
})
```

And we're done!

## RTL / LTR

While writing this article, I found out that Chris Coyier solved the same problem [3 years ago](https://css-tricks.com/star-ratings/) (Doh, I'm so slow!) by setting `unicode-bidi` to `bidi-override` and `direction` to `rtl`. These two settings combined achieves the same effect as reversing the rendering order of elements like `row-reverse`.

So, the biggest credit still goes to Chris for creating this method so many years ago! :)

The only benefit my Flexbox method provides over Chris's method is that stars will automatically face the correct direction if you set the direction to `rtl` or `ltr`.

<p data-height="266" data-theme-id="7929" data-slug-hash="YwjZQv" data-default-tab="result" data-user="zellwk" class='codepen'>See the Pen <a href='http://codepen.io/zellwk/pen/YwjZQv/'>Ratings with Flex (RTL)</a> by Zell Liew (<a href='http://codepen.io/zellwk'>@zellwk</a>) on <a href='http://codepen.io'>CodePen</a>.</p>

## Wrapping Up

And thats all the code you'll ever need for a rating element. It's pretty cool to build this UI with so little code, isn't it?

<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
