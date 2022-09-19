# druber-responsive

## 📝Note before reading:
To help illustrate my answer, I've created a (*very*) basic landing page for DRUBER. Please feel free to take a look at the source code above (the focus was *not* on the design, but on making the page mobile friendly). 

## 🤒 Issue

"You and your friends are developing a new start-up called DRUBER, a drone-based ride share application that carries you to your destination. The original specification was to develop a web page that works in 1920x1080 however your company has realized that it is missing an entire market of smartphone users. Describe how you can modify your code to work in smartphone resolutions e.g. 750x1334 (iPhone 8). Please give specific examples where possible, but do not implement an entire DRUBER clone!"

## 📱 Why is mobile compatibility important? 

> Globally, 68.1% of all website visits in 2020 came from mobile devices—an increase from 63.3% in 2019.

When was the last time you called an Uber from your laptop? You've probably never done that before! Similarly, most customers will likely be interacting with DRUBER through their phones, and it's absolutely critical to accomodate to these mobile users. Poor web design drives users away and increases bounce rates – in order to fully capture our market, we must create a seamless experience for all customers, on all devices. 

## ✅ What is my solution? 

📱Responsive design using media queries. 

### ❓What is responsive design? 

Responsive design accounts for the size of the user's screen and adjusts the display of content accordingly; this allows for compatibility with all devices. 

### ❓Why media queries?

Media queries will allow the DRUBER team to style elements based on viewport width. 

## 🤷🏻‍♀️ Where do we start? 
1. Before diving into the code, it's crucial to understand that this issue impacts *both* developers *and* designers. Before diving head first into the code, it's a good idea to work together **to create a separate wireframe** for mobile. This will allow the team to collectively work through the structure of the website and establish expectations. 
2. Once the wireframe has been completed, I would delegate the pages to ensure full coverage and avoid overlap. This will also help us to prevent merge conflicts and to work at a faster pace. 

## 🧩 Solution Implementation + Example
To illustrate the process of making DRUBER mobile friendly, I will be using a mock landing page as an example. Right now, it is only compatible with desktop (1920x1080). As shown below, the webpage becomes extremely unreadable and poorly formatted as the size of the screen narrows. 

![Not Responsive](https://user-images.githubusercontent.com/73853606/190954655-3bb91005-a0a7-435f-ab0a-13de7062055d.gif)


### ‼️Important 

First and foremost, I must make sure that there is a "viewport" `<meta>`tag to control the viewport's size and shape: 

`<meta name="viewport" content="width=device-width, initial-scale=1" />`

This will ensure that the browser is aware of the device's dimensions. 

### ✂️Split into sections
Next, I notice that this page has three distinct sections: 
1. The navigation bar at the top
2. The "About" section with images/text 
3. The "Order" section with user input dialog box

I'm going to tackle each section one at a time, starting with "About" as it seems to be the most simple. 

### 🔨 Fixing the "About" Section
    
This section features side-by-side images with descriptions – a very basic layout seen on most websites. To make this mobile friendly, we simply have to ensure that the images/descriptions stack on top of each other (or in other words, wrap) when the width of the screen becomes narrow enough. 

🧠 Which part of the CSS is telling these images to be aligned horizontally? 

💡 It looks like it's `flex-direction` being set to `row`. 

✅ Solution: Add a media query.

```
@media (max-width: 800px) {
    .descriptions {
        flex-direction: column;
    }
}
```
Explanation: 
`@media (max-with: 800px)` is esentially saying: "As long as the screen is less than 800px..."

This allows us to change the styling of the section when the screen width is less than 800px. 

The images and descriptions are now stacked on top of eachother, but it doesn't look quite right. This is because the width of the images are still set to `33%`, which no longer makes sense for mobile as the images are on separate rows. Thus, I can modify the width and height accordingly to: 

```
@media (max-width: 800px) {
    .descriptions>div {
        width: 100%;
        height: 30%;        
    }
}
```
### 📱 Responsive "About" Result
![About Responsive](https://user-images.githubusercontent.com/73853606/190954191-e90dbd97-129b-46fb-90f7-da831b6670fc.gif)


## 🔨 Fixing the Navigation Bar 

In responsive design, some sections cannot just be rearranged, but rather have to be completely reimagined. The "About" section above was fairly simple as we merely had to ajdust the positioning and size of the elements. On the other hand, a component like the navigation bar is more challenging. The navigation menu items will clearly not fit onto the header of a mobile screen. 
    
🧠 What should we do with the menu links?  
💡 We can collapse all of the items into a hamburger menu that expands on click. 

1. Firstly, I'll make a toggle button by inserting the following into my HTML page: 

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
Notice `display: none`. This has been added intentionally as the hamburger button *should not* show on desktop. This is a very useful technique to hide certain elements when designing for mobile.

I'll now use a media query to ensure that the hamburger button *is* displayed when the screen width is narrow enough: 
 
```
@media (max-width: 800px) {

    .toggle-button {
        display: flex;
    }
```
When the toggle button appears on mobile, the navbar items should disappear. I can achieve this by setting `display: none` (within that same media query). 

In order to determine whether the menu items should be shown or not, I will use `.active` to display the menu items when they are active:
    
  ```
    .navbar-links.active {
        display: flex;
    }    
```

The rest of the CSS in this media query is to style the menu bar: 
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
```
    

Finally, I'll have to add some Javascript in order for the toggle button to actually open and close. In my script.js file, I add: 

```
const toggleButton = document.getElementsByClassName('toggle-button')[0]
const navbarLinks = document.getElementsByClassName('navbar-links')[0]

toggleButton.addEventListener('click', () => {
    navbarLinks.classList.toggle('active')
})
```

### 📱 Responsive Nav Bar Result
![Responsive Nav Bar](https://user-images.githubusercontent.com/73853606/190954160-c6ccaef0-a756-444e-a189-df7d36ec9037.gif)

    
## 🔨 Fixing the "Order" section
To avoid repetition, I will not walk through restyling this section as it is very similar to the layout change in "About". 

## ⚠️ Important Considerations

Throughout the process of improving the mobile experience, it is worth asking the following questions: 
    
- **Prioritization:**
  A smaller device means content is fighting for screen space – which sections of the website are the most important and add the deepest value to our       customers?  
- **Performance:**
  The performance (speed at which content loads and renders) of a website is strongly correlated to user experience. Our responsive website will attract     high traffic from both desktop and mobile users. How can we finetune and optimize our website's CSS performance and deliver content faster to our         customers? What best practices can we follow? 
- **Accessibility:**
  How can we ensure that our website design and layout is accessible to all people? Are we accommodating to those with health conditions/impairments? How   can we seek to provide a positive, intuitive experience for *all* visitors? 
- **Modularity:**
  How scalable is our website? Have we broken it up into modules that can be easily and flexibly rearranged? Do we have reusable components and             patterns? 
- **Testing:**
  Have we extensively tested our website on various devices to minimize bugs and ensure there is no unexpected behaviour? 

## 🤔 Finally, what *should* the DRUBER team have done from the start?
    
If we could go back in time, the DRUBER team should have first prioritized mobile design, then worked our way up to more complex, desktop screen sizes. Mobile-first design (also known as progressive advancement) is important as it naturally forces developers/designers to understand and identify the most necessary and key elements of a website, without the added fluff. Prioritizing these critical sections and making sure they are first and foremost properly displayed is an effective approach to web design. If we had first nailed down our mobile site, we could have then added more complexity and functionality to the desktop version afterward. 
