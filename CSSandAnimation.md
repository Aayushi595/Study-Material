Every HTML element is a box, and its total size is made of: content + padding + border
Default browser behavior → box-sizing: content-box . Width/height apply only to content, NOT padding or border.
    box-sizing: border-box . width/height includes everything.   content shrinks automatically to fit padding + border
    
# Height - Width
1. percentage - relative to parent 
2. vw, vh - relative to viewport
3. pixels - absolute (fixed)
4. auto - Expands to available space

min-width -
max-width -
overflow-hidden - Applied on Parent. This is not exact solution: extra part is clipped, some childs can remain even invisible.

for fonts - em (relative to parents font size), rem (Relative to root (html) font-size)
1rem = 16px (by default in browsers)

# Display props - width - height

1. Block Element/display : block — starts on new line. 
Width : stretches full width of its parent 
Height : without a specified height they will take the height of its content.
Relative Height : 80% 
     If the parent has explit height : 500px, and childs are 80%, then childs simply go out of container - no wrapping .

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
    
3. Inline-block : Sits side by side (like inline), takes explicit width and height.
Note => For taking explicit width, they can behave from inline to block.
Try Ex:
   <div style="border:1px solid green;">
        <div class="text" style="width : 80%;display: inline-block;">text 1</div>
    </div>


4. Display:flex - Makes a container(parent) as Flexbox. 
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
    
    Align-self overrides align-item

    Flex properties - Children
    1. flex : (how much space a child will occupy, relative to other childs, when extra space is available)
       flex : 1 {shorthand for flex-grow , flex-shrink}

    2. flex-grow : Defines how much a flex item grows relative to others, if extra space is available.
       default value : 0 (cannot grow until set to 1)

    3. flex-shrink : We don’t need to explicitly pass flex-shrink: 1 because. It is the default value for flex items.

5. inline-flex : Makes element inline on the outside / flex in the inside 
ex : <span class="box">
      <span>1</span>
      <span>2</span>
    </span>


# Focus Events:
    css also allows focus event.
    input:focus, button:focus, div:focus {
    outline: 3px solid blue;
    background-color: #eef;
    }



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

# CSS Gyan

- object-cover 
Background banners full width - Fill the entire container (both width and  height), keep aspect ratio, crop extra parts if needed.
Ex - <div className="absolute">
        <img className="w-full h-screen object-cover" />
    </div>
Inside this image box, adjust the image content. No distortion



- inset-0 - works only if the element is absolute, fixed, relative .
top: 0; right: 0; bottom: 0; left: 0;







 
