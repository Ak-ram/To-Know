# Responsive Layouts 

In css there are significiant ways to create a responsive layouts 
-  Float (old school)
-  Flexbox
-  Gridbox

IN this article we will take about Float way, but why i will do that ? it's old and no one use it right now, let me illustrate that major people migrate to use flexbox and grid but there are some tools that depend on it's implementation on old css styles and it knnow nothing about new features like flexbox and grid, one of this tools that i face in my work is openHtmltoPdf tool that used to convert html/css to pdf, so without go ahead anymore in this answer, let's get started.

## Document Flow
before talk about what is floating we should first know what **document flow** is ?
document flow is about the arrangement of the **block** html elements in **one** level ! we can represent block element in a cubic shape like this:


![إضافة عنوان (3)](https://github.com/user-attachments/assets/4ec49c38-54f0-4580-9299-44bd22a2569d)

The image above illustrate that everything exist in our block element `div` in our case, will be live in container plane by default.


# Question ? But why we don't see these cubics in our screen ? 

I prepare this image to illustrate this point, it's out of our scope but i hope you get the answer from it.

![إضافة عنوان](https://github.com/user-attachments/assets/f4f9ff53-90b1-4df9-8a9e-da40b7c0d4d2)

# Another Question ? Can elements jump from container plane to another plane ? 
### YES, And this is what css `float` does

Float property make element's transfer from container plane to the corresponding plane, see this image 


![إضافة عنوان](https://github.com/user-attachments/assets/93e900f4-d412-4b56-a77b-ac8774a66809)

## Floating

we can achieve this floating by using float property 

```css 
  float: left ; /* left || right */
```
  
> [!NOTE]
> `float` property affects also on `display` property of the element, as i convert **block** elements to **inline** one.
> Floated element doesn't leave a gap in the container layer as the next elements `.p1` & `.p2` auto fill this gap, revisit the above image again.
