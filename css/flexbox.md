# Flexbox

[A Comple Guide To Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
[Flexbox - Can I use](http://caniuse.com/#feat=flexbox)
[Flexbox Froggy](http://flexboxfroggy.com/)
[Using CSS flexible boxes](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes)

## enable on container

`display: flex;`    enable flexbox


## horizontal positioning

`justify-content: flex-start;`      left align
`justify-content: flex-end;`        right align
`justify-content: center;`          center align
`justify-content: space-between;`   equal spacing between
`justify-content: space-around;`    equal spacing around

## vertical positioning

`align-items: flex-start;`  align to top
`align-items: flex-end;`    align to bottom
`align-items: center;`      align to middle (center)
`align-items: baseline;`    align to baseline
`align-items: stretch;`     stretch items to fit container

## display direction

`flex-direction: row;`              placed the same as text direction
`flex-direction: row-reverse;`      placed opposite to text direction
`flex-direction: column;`           placed top to bottom
`flex-direction: column-reverse;`   placed bottom to top

## horizontally position an individual item

`order: 1;`     change order value to 1
`order: -1;`    change order value to -1

## vertically position an individual item

`align-self: flex-start;`   align to top
`align-self: flex-end;`     align to bottom
`align-self: center;`       align to middle (center)
`align-self: baseline;`     align to baseline
`align-self: stretch;`      stretch items to fit container


## adjust item wrapping to new lines

`flex-wrap: nowrap;`            fit even item into single line
`flex-wrap: wrap;`              wrap items to additional lines
`flex-wrap: wrap-reverse;`      wrap items to additional line in reverse


## combination of flex-direction and flex-wrap

`flex-flow: (flex-direction-property) (flex-wrap-property)`


## adjust spacing of multiple lines

`align-content: flex-start;`        packed at top of container
`align-content: flex-end;`          packed at bottom of container
`align-content: center;`            packed at vertical center of container
`align-content: space-between;`     display with equal spacing between
`align-content: space-around;`      display with equal spacing around them
`align-content: stretch;`           stretched to fit the container


## example usage

```css
.container {
    display: flex;
    align-content: center;
    flex-direction: column-reverse;
    flex-wrap: wrap;
}
```
