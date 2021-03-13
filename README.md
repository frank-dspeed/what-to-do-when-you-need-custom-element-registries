# what-to-do-when-you-need-custom-element-registries
This Repo Shows what to do when you need custom elemen registries


## What is a CustomElement Registrie?
Its a Object that exists in current browsers that allowss to define some elements that should get loaded diffrent when parsed or inserted into the DOM

here a example - This is called "imperative" Code pattern 

```js
customElement.define('my-component', class{})
const newEl = document.createElement('my-component')
document.body.append(newEl)
```

### Short explainer imperativ vs declarativ. 
- Imperativ direct instruction to do something and it gets done in that moment
- Declarativ is when you declare a definition of something for example inside html you could declarativ say load script that calls customElement.define <script></script> and later you put into the same html declarativ the content <m<-element></my-element>

```html 
<html><head>
<script src="define-custom-elements.js"></script>
</head><body>
<my-element></my-element>
</body></html>
``` 

## Declarativ ShadowDOM & CustomElement Registries.
This are two Proposals that blow my mind because i think they lead to unbelive able behavior of code and should get avoided at any cost. let me show you the problem that this proposals try to solve

```js
customElement.define('my-component', class{})
// in a other script file
customElement.define('my-component', class{}) // This Errors
```

```html
<html><head>
  <script src="define-custom-elements.js"></script>
  <script src="define-custom-elements-from-others.js"></script>
</head><body>
<my-element></my-element>
<!-- div tag here is out of our control lets say gets rendered by lit-html or something else -->
<div>
  <my-element></my-element>
</div>
</body></html>
```

## Legit existing Solutions
- Do never use customElements that depend on TagNames and simply register the customElements definition under a diffrent tag name and declare the new tagName in your html.

now lets assume we got no bundler and other-script.js is a 3th party code that we do not want to touch

```js
customElement.define('my-component', class{})
// in a other script file
customElement.define('my-component', class{}) // This Errors
```

## Arrguments against customElement registries
We can not clear and easy as a coder say what element gets rendered in which state of the render process. same problem as with inharitance

## What do customElements do?
- They define the way a DOM node with a given tagName or "is" attribute gets inserted and removed.
- They can get polyfilled by Mutation Observe, a scriptTag inserted near the element or a big combination of imperative ways
  - cleanups can also be done imperativ via Mutation Observer or WeakMaps.


## Is customElements define a general use API?
If you want to create a Application based on custom-elements that you load and then for example in a online editor to create custom views?
 - Yes that is possible and desired this way you can create encapsulated Components that work well together
 - No If you got 3th party components that define Tags that you want to combine then.
 - Yes if you got 3th party ElementDefinitions (only the class) and that is not referencing other custom elements by tagName
 - Yes if you got 3th party ElementDefinitions and do a custom rewrite logic like customElement registries that are scoped
If you want to create a Element with custom behavior at a single place where you do not know the tag before your better with <script> tags that apply customElement definitions at the right render step
  
  
  
  
  
#### Here i should link a video Example
How to fine grained control DOM Handling and Rendering Process.
- Show HTML Component Pattern (Tag+scriptTag)
- Explain diffrent Browser Rendering Steps and Hooks. Ref: Google material about diffrent script tag behaviors and dom events.
- Show MutationObserver
- Show hireOrderFunctions for Element Creation.
- Explain CustomElements.defines implementation how it Hooks into the DOM Handling
