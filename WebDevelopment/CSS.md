### Selectors

From most specific to least:

+ !mportant
+ Inline: `<p style='color:orange'></p>`
+ Id: `#logo`
+ class: `.heading`
+ type: `p`, attribute: `a[href="https://google.com"]`
+ universal: `*`

Complex selectors:

+ list: `#main, .content, div{}`
+ descendent: `ul li{}`
+ child: `div > span{}`
+ general sibling: `p ~ span{}`
+ immediate sibling: `p + span{}`

Pseudo selectors

+ Pseudo classes: `:`, `button:hover` `:active` `:focus`
+ Pseudo elements: `::`, `p::before`, `::after`

### Box Model

+ Margin, border, padding, content
+ Standard box: w, h-> content box
+ `box-sizing: border-box;`: w,h-> border box

### Flexbox/ grid

+ Horizontal align
+ Vertical align

### Media Queries

### Positioning

+ Static
+ Relative
+ Absolute
+ Fixed
+ Sticky

### Units

+ Absolute
+ Relative
  + ems
  + rems