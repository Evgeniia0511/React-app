# React App TODO

## 1. Folder structure

Usually we organize an app like this. Try to copy this folder structure when you are adding components. Ignore the `__test__` folders for now... we will do tests later, but this is why we keep everything in folders.

```
src/
    components/
        App/
            __tests__/index.test.js
            index.js
        Header/
            __tests__/index.test.js
            index.js
        Image/
            __tests__/index.test.js
            index.js
        index.js
    index.js
public/
package.json
```

In `src/components/index.js` we provide exports for all components in one file...

```JavaScript
export { default as App } from './App'
export { default as Header } from './Header'
export { default as Image } from './Image'
```

And when you need to import a component you can do something like...

```JavaScript
import { Header, Image } from 'src/components'
```

Here's an example of what the Header componet should look like:

```JavaScript
import React from "react"
import { string } from "prop-types"

const headingStyle = {
    color: '#303030',
    fontSize: '26px',
    fontWeight: 'normal'
}

const subHeadingStyle = {
    color: '#606060',
    fontSize: '18px',
    fontWeight: 'normal'
}

class Header extends React.PureComponent {
    static propTypes = {
        heading: string.isRequired,
        subHeading: string
    }

    render() {
        const { heading, subHeading } = this.props
        return (
            <header>
                <h1 style={headingStyle}>{heading}</h1>
                {subHeading && <h2 style={subHeadingStyle}>{subHeading}</h2>}
            </header>
        )
    }
}


export default Header
```

Notice that this component extends `PureComponent`. Important to read about the difference between `Component`, `PureComponent`, and components that are functions `(props) => { return <div>Hello</div>; }`.

So, setup your app into a similar structure and add the Header compoent and use it in your main App component.

## 2. Image component

Next I want an image component that I can give a specific width and it will keep specific proportions. Don't use any resize events, use only CSS rules and calculations in JS if needed. Render as a background image so that we can automatically crop when the image doesn't fit. I want to be able to use it in my components like this:

```JavaScript
const imageSrc = "./kitten.jpg"
const width = "50px"
const aspectRatio = "4:3"

<Image
    imageSrc={imageSrc}
    width={imageWidth}
    aspectRatio={aspectRatio}
/>
```

## 3. Square component

In our game board we need not just images, but buttons that are clickable. So next you should make a button component called `Square`. This component should keep an internal state called `value` that can be either `null`, `'X'` or `'O'`. Make the button clickable and add an event handler, that updates the state. The empty state should be `null`, and each time you click the button it cycles through the possible states, `null -> 'X' -> 'O'` and then restarting. Based on the current state, render a different image as the contents of the button, using our Image component.

```JavaScript
class App extends React.Component {
    render() {
        return (
            <div>
                <Header heading="Tic-Tac-Toe" />
                <div>
                    <Square />
                    <Square />
                    <Square />
                </div>
                <div>
                    <Square />
                    <Square />
                    <Square />
                </div>
                <div>
                    <Square />
                    <Square />
                    <Square />
                </div>
            </div>
        )
    }
}
```

You can add some styles to make it look nicer. Next you will make another component for the game board that will be wired up to these squares.

## 4. Board component

Right now every square component is tracking its own state interally. For the game, however, we should track all of the game state in one place. This will be in the `Board` component. After you have completed the game using plain React, we will update the game to instead track the state using Redux.

Here is a possible starting point for this component... edit or add to it as you need. In general, the `Board` should render a grid of `Square` components, using `this.state.board[n]` to determine their props. Each `Square` component also needs a way to tell the `Board` when it is clicked and let it update the state.

```JavaScript
class Board extends React.Component {
    state = {
        board: new Array(9).fill(null),
        // ...
    }

    handleSquareClick = () => {
        // ...
    }

    render() {
        // ...
    }
}
```

In general the game will flow like this:
* Render game status (squares + current player), using current state
* Player clicks on a square and triggers `onClick` handler
* Handler function  on `Board` is called
* Validate if the player move is correct, and update `Board` state if necessary
* When state is updated, React automatically re-renders  using the new state.
* Repeat...
