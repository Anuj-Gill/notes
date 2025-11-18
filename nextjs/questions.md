#nextjs 

#### Why use client when using react hooks?
Server components use node runtime, react hooks and handlers require client-side runtime, browser instances which are not available in node runtime

#### What is Hydration
Hydration is the action of adding interactivity to our pre-rendered HTML pages.

#### How the browser renders any webpage (React or non-React)

When you visit a webpage, the browser does three main things:

1. **Downloads HTML**  
    HTML is parsed from top to bottom, line by line.  
    As it parses, it builds the DOM tree and starts painting things on the screen.
2. **Downloads CSS**  
    CSS is needed to style the page.  
    The browser may delay rendering certain parts until CSS is fetched, because it needs styles to compute layout.
3. **Downloads JS**  
    JS can modify the DOM, add interactivity, run logic.  
    JS does not render HTML by itself (not initially).  
    It only _changes_ the DOM that HTML created.

This flow (HTML → CSS → JS) is the **browser’s rendering flow**, not React’s.

#### Hows does React render a page on browser
1. Get's the intial HTMl: just has a empty root div
2. Get's the CSS then: <link rel="stylesheet">
3. Get's the JS bundle which has:
	1. JS Logic
	2. React lib
	3. Components
4. React turns your components into an in-memory tree.  
This is not HTML.  
It’s a JS object that represents what the UI should look like.
EX: 
```
{
  type: 'div',
  props: { className: 'card' },
  children: [
    { type: 'h1', children: ['Hello'] },
    { type: 'button', children: ['Click'] }
  ]
}
```
5. React turns the Virtual DOM into real DOM nodes
	React uses normal browser JS functions like:
```
document.createElement()
element.appendChild()
```
6. Browser renders the final DOM to screen

There after, on every update, it creates a virtual DOM, compares with the rpevious one and then updates the changed element only with using the JS.