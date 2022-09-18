# druber-responsive

## Issue

"You and your friends are developing a new start-up called DRUBER, a drone-based ride share application that carries you to your destination. The original specification was to develop a web page that works in 1920x1080 however your company has realized that it is missing an entire market of smartphone users. Describe how you can modify your code to work in smartphone resolutions e.g. 750x1334 (iPhone 8). Please give specific examples where possible, but do not implement an entire DRUBER clone!"

## Why is mobile compatibility important? 

## What is my solution? 

Responsive design! 

### ❓What is responsive design?

Responsive design accounts for the size of the user's screen and adjusts accordingly; this allows for compatability with all devices. 

## Assumptions
Although this solution can be widely applied across different languages and frameworks, I have chosen to use vanilla HTML/CSS for the purposes of this demonsration. 

## How will my solution be achieved? 
1. Assuming that the Druber website has multiple pages, I would first delegate pages to the frontend team members to ensure full coverage and avoid duplication 
2. Once I know which page(s) to work on, I would work on the associated styling sheet(s) and use media queries. 

### ❓Why media queries?

Media queries will allow me to style elements based on viewport width. 

While I could create another stylesheet for different media, I am going to inject media queries directly into the already-existing style sheets for simplicity. I would definitely considering separating them out if this website was more complex (for readability). 

## Solution implementation and example 
To illustrate how I would make this website mobile-compatible, I have created a (*very*) basic (and unattractive) landing page for Druber that is only properly displayed in desktop (1920x1080). As shown below, the website becomes extremely unreadable and poorly formatted as the size of the screen narrows. 

<Insert GIF of website> 

### Where do I start? 

First and foremost, I must make sure that there is a "viewport" `<meta>`tag to control the viewport's size and shape: 

`<meta name="viewport" content="width=device-width, initial-scale=1" />`

This will ensure that the browser is aware of the device's dimensions. 

Next, I notice about this page is that there are three main sections: 
1. The navigation bar at the top
2. The "About" section with images/text 
3. The "Order" section with user inputs 

I'm going to tackle each section one at a time, starting with the "About" section as it seems to be the most simple. 

### Fixing the "About" section

I've chosen to demonstrate ___ because I garauntee you will come across this kind of layout in any webpage. This fix will be quite simple, as all we have to do is to ensure that the images can stack ontop of each other (also known as wrap). 

Think: what part of the CSS is causing these images to render in one row? 

I notice that `flex-direction` is set to `row`. 

I add a media query: 

```
@media (max-width: 800px) {
    .descriptions {
        flex-direction: column;
    }
}
```
What is this doing? 
`@media (max-with: 800px)` is esentially saying: "As long as the screen is less than 800px..."

This allows us to change the styling of the "About" section when the screen size is less than 800px. 

The images and descriptions are now stacked on top of eachother, but it doesn't look quite right. This is because the width of the images are still set to `33%`, which made sense when they were sharing the width of the desktop screen, but not anymore. I modify the width and heights accordingly to: 

```
@media (max-width: 800px) {
    .descriptions>div {
        width: 100%;
        height: 30%;        
    }
}
```

## Fixing the nav bar 

In responsive design, it's not always about figuring out how to rearrange components to make them look presentable, but it's also important to identify sections that need to be re-designed. The "About" section was very simple to change as only the positioning and size of the elements were moved around. A component like the nav bar is more challenging as its appearance will be changing.

It doesn't take long to realize that it is very unlikely that all of the navigation items will fit on a mobile screen. What can we do instead? We can collapse all of the items into a hamburger menu that expands on click. 

1. Firstly I'll make a toggle button by inserting the following into my HTML page: 

```
   <a href="#" class="toggle-button">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
   </a>
```
Next, I'll add some styling: 

```
.toggle-button {
    position: absolute;
    top: .75rem;
    right: 1rem;
    display: none;
    flex-direction: column;
    justify-content: space-between;
    width: 30px;
    height: 30px;
}
```
Notice `display: none`. This has been added intentionally as the hamburger button *should not show* on desktop. This is a very common technique when designing for mobile.

Now, I'll use a media query to ensure that the hamburger button *is* displayed when the screen width is narrow enough: 

```
@media (max-width: 800px) {

    .toggle-button {
        display: flex;
    }
```
When the toggle button appears, we should also make the navbar links disappear. I can achieve this by setting `display: none` (within that same media query). 

The rest of the css in that media query is to style that menu bar. It should look like: 

```
   .navbar {
        flex-direction: column;
        align-items: flex-start;
    }

    .navbar-links ul {
        width: 100%;
        flex-direction: column;
    }

    .navbar-links ul li {
        text-align: center;
    }

    .navbar-links ul li a {
        padding: .5rem 1rem;
    }

    .navbar-links.active {
        display: flex;
    }
```
Lastly, I'll have to add some Javascript in order for the hamburger button to toggle open. In my script.js file, I add: 

```
const toggleButton = document.getElementsByClassName('toggle-button')[0]
const navbarLinks = document.getElementsByClassName('navbar-links')[0]

toggleButton.addEventListener('click', () => {
    navbarLinks.classList.toggle('active')
})
```



## Who does this impact? 

## Challenges to consider

## What *should* the Druber team have done?

## Future considerations
