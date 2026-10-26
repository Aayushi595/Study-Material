# CSS fundamentals

* Box Model
Every HTML element is a box, and its total size is made of: content(inner most) + padding + border (outer most)
Default browser behavior → box-sizing: content-box . Width/height apply only to content, NOT padding or border.
    box-sizing: border-box . width/height includes everything.   content shrinks automatically to fit padding + border
  Margin — outside the border . Always added , nothing to do with box-sizing prop.
    
* length units in css
1. percentage - relative to parent 
2. vw, vh - relative to viewport
3. pixels - absolute (fixed)
4. auto - Expands to available space


* Display props and width - height

1. Block Element/display : block — starts on new line. 
Width : stretches full width of its parent 
Height : without a specified height they will take the height of its content.
Relative Height : 80% 
     If the parent has explit height : 500px, and childs are 80%, then childs simply go out of container - no wrapping .
<div>  <p>  <h1>-<h6>  <section>  <article>  <header>  <footer>
<nav>  <main>  <aside>  <ul>  <ol>  <li>  <form>  <table>
<blockquote>  <pre>  <hr>

2. Inline Element / display : inline -- starts in same line. 
Width : Take up only as much width as their content. 
Height : Of its content, affected by line-height. - cannot set width/height using css.
  Explicit width/height - Inline elements completely ignore height (and width).They are designed for text flow.

Note => Inline Parent + Block childs = Illogical/invalid layout.
Try Ex: 
  <div style="border:1px solid green;display:inline">
        <div class="text">text 1</div>
    </div>
     <div style="border:1px solid blue;display:inline">
        <div class="text1">text 1</div>
    </div>

<span>  <a>  <strong>  <em>  <b>  <i>  <u>  <small>  <mark>  <sub>  <sup>  <abbr>  <code>  <label>  <time>
    
3. Inline-block : Sits side by side (like inline), takes explicit width and height.
Note => For taking explicit width, they can behave from inline to block.
Try Ex:
   <div style="border:1px solid green;">
        <div class="text" style="width : 80%;display: inline-block;">text 1</div>
    </div>

4. Display : flex .

5. inline-flex : Makes element inline on the outside / flex in the inside 
ex : <span class="box">
      <span>1</span>
      <span>2</span>
    </span>


  - line-height : height of text

  - min-width : work with normal block layout, Flexbox, Grid, and other layout systems.
  - max-width : work with normal block layout, Flexbox, Grid, and other layout systems.

  for fonts - em (relative to parents font size), rem (Relative to root (html) font-size)
  1rem = 16px (by default in browsers)

* Overflow Props. 
  Applied on parent, when child exceeds parent's area. Parent need not to be a flexbox. 
  - overflow : hidden - Excess content is hidden; no scrollbars.The clipped content still exists in the DOM.
  - overflow : auto - Scrollbars appear when needed.
  - overflow : scroll - Provides scrolling even if it isn't needed; scrollbar visibility varies by OS.
  - overflow : visible -  Overflow is visible outside the box. Default behavior.



# CSS Layout 
  
  # Flexbox
  Display:flex - Makes a container(parent) as Flexbox. Objective : Create responsive layouts that adapt to different screen sizes.
   One dimensional layout.
   Note => children streches (by default) in the cross axis, if explicit dimensions are not given.
   a. flex-direction : row (by default for browser), column (by default in mobile apps), row-reverse (mirror image of flex-direction:row for that container like a book is closed and content printed from left to right side), column-reverse (book closed up to down, content printed rom upper to lower page.)
   b. justify-content : center, flex-start, flex-end, space-between, space-evenly, strech (works only if height/width is not fixed)
      align-items : center, flex-start, flex-end, space-between, space-evenly
      
      main axis = justify-content = flex-direction, cross-axis = perpendicular to main axis

    c. flex-wrap : Controls whether flex items wrap (accomodate in next line) or not when they overflow/shrink the container.
       wrap, no-wrap(by default), wrap-reverse.
       flex-direction : row, flex-wrap: wrap-reverse : last row on top.
       flex-direction : column, flex-wrap: wrap-reverse : last column row at first.

    d. flex-flow : flex-direction + flex-wrap. eg. {flex-flow : row wrap}
    

    Flex properties - Children
    1. flex : (how much space a child will occupy, relative to other childs, when extra space is available)
       flex : 1 {shorthand for flex-grow , flex-shrink}

    2. flex-grow : Defines how much a flex item grows relative to others, if extra space is available.
       default value : 0 (cannot grow until set to 1)

    3. flex-shrink : We don’t need to explicitly pass flex-shrink: 1 because. It is the default value for flex items.

    4. align-self : align-self overrides align items for that specific children to which this css is applied. Values : center, flex-start, flex-end, space-between, space-evenly


  # Grid


# positioning props 
top, right, bottom, and left - used to offset an element from a reference point. UNits - css length units.

static — default positioning. Fixed at their place. top, right, bottom, left don't work.
relative — offset from its normal position.
absolute — positioned relative to its nearest qualifying positioned parent.
   - positioned parent : position value of parent is anything other than static.
fixed — positioned relative to the viewport in the usual case. 
sticky - behaves relatively until a scroll threshold is reached, then sticks within its scrolling container.
Ex : .header {
  position: sticky;
  top: 10;
}

inset : 0 : sets all four inset properties to zero. top : 0, left : 0, right : 0, bottom : 0. Commonly used with absolute positioning to make an element cover its containing block.

# CSS Stacking / Layering
- z-index :
Works on positioned elements (relative, absolute, fixed, or sticky). But Flex items and grid items can use z-index without setting position.

# CSS Replaced Elements
 whose content or appearance is supplied by an external resource. <img> <video> <audio> <iframe> <object> <embed> <input>

* CSS image/video sizing and positioning
- object-fit: cover - Fills the box; some image content may be cropped. Used with replaced elements to fill a fixed-size box without distorting the image's aspect ratio. With other tags -  no effect.
- object-fit: contain - Shows the entire image; empty space may remain.
fill — fills the box, potentially distorting the image. This is the default.
- object-position controls which part of the image is aligned within the box especially when object-fit: cover crops the image. Like face in the image will be at top. Eg. object-position: center top;

# 5. Typography and visual styling
font-family, font-size, font-weight
line-height, letter-spacing, text-align
text-overflow, white-space, overflow-wrap
text-decoration, text-transform
Background images and gradients
Borders, outlines, shadows
opacity, visibility, display: none
cursor, appearance, accent-color
Pseudo-classes: :hover, :focus, :active, :disabled
Pseudo-elements: ::before, ::after, ::placeholder


# Responsive design
Media queries: @media
Mobile-first design
Breakpoints
Fluid widths and responsive spacing
%, max-width, min(), max(), clamp()
Responsive Flexbox and Grid
Responsive images
Viewport units: vw, vh, dvh
Handling horizontal overflow
Responsive navigation, cards, forms, and dashboards


# Transitions, transforms, and animations
transition, transition-duration, transition-timing-function
transform: translate(), scale(), rotate()
@keyframes and animation
Hover effects and dropdown animations
Loading spinners and skeleton animations
prefers-reduced-motion
Performance considerations for animations

# Forms and accessibility
Styling input, textarea, select, button
:focus-visible and keyboard focus indicators
:checked, :disabled, :invalid
outline vs border
Accessible contrast and readable text
Native form controls vs custom controls
Responsive form layouts

# Advanced CSS and maintainability
CSS variables: --primary-color, var()
CSS nesting and @supports
calc(), min(), max(), clamp()
CSS modules and scoped styling in React
BEM naming convention
CSS resets and normalization
Cascade layers: @layer
Container queries: @container
Logical properties: margin-inline, padding-block
Specificity debugging and browser DevTools


# Tailwind CSS
h-screen : 100vh => 100% of viewport height
w-full : full width of parent
-z-10  : z-index : -10

- grouped css using class and id
1. layer - @apply allows tailwaind utilities to be used on normal css classes.
    login.css -
    @layer components {
    .login-form {
        @apply w-full md:w-5/12 lg:w-3/12 absolute p-12 bg-black 
            top-1/2 -translate-y-1/2 mx-auto right-0 left-0 
            text-white rounded-lg bg-opacity-80;
    }}

    use in Login.js -
    import './Login.css';
    <form className="login-form"></form>

2. css modules
Ex -
  import styles from './Login.module.css';
  <form className={styles.form}></form>
  
  Login.module.css :
     .form {
    width: 100%;
    padding: 3rem;
    background: rgba(0, 0, 0, 0.8);
    position: absolute;
    top: 50%;}

3. styled components
4. Material UI

- Global level custom breakpoints :
In tailwind.config.js  : 
    theme: {
  extend: {
    screens: {
      'custom': '880px'
    }
  }
}

// Option 2: OVERRIDE (replaces default breakpoints)
theme: {
  screens: {
    'sm': '640px',
    'custom': '880px',
    'lg': '1024px'
  }
}

- Element level custom breakpoints.
  1. [880px]:w-4/12
  2. media/query using class and id - implemeted in css modules (Login.css)/ layer
  3. Styled-components








 
