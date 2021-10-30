![Crooks](https://raw.githubusercontent.com/chrisjpatty/crooks/master/logo.png)

> Generated by [create-react-hooks](https://github.com/use-hooks/create-react-hooks).

## Available Hooks

- [useLocalStorage](https://www.github.com/uvacoder/hook-hooker#useLocalStorage)
- [useFiler](https://www.github.com/uvacoder/hook-hooker#useFiler)
- [useKeyboardShortcut](https://www.github.com/uvacoder/hook-hooker#useKeyboardShortcut)
- [useOnClickOutside](https://www.github.com/uvacoder/hook-hooker#useOnClickOutside)
- [imageSize](https://www.github.com/uvacoder/hook-hooker#imageSize)
- [usePermissions](https://www.github.com/uvacoder/hook-hooker#usePermissions)
useClientHydrated
useElementSize
useFocusTrap
useId
useInteraction
useLazyRef
useMedia
useNativeLazyLoading
useScript
useToggle
useWindowSize

useListenAlong https://github.com/punctuations


# useLocalStorage

`useLocalStorage` behaves just like the native react `useState` hook, except that any and all state updates are automatically saved in the browser\'s localstorage under the provided key. The first argument is the name of the key to save it under, and the second argument is the initial value. The hook returns the current state and an updater function just like `useState`.

When the app reloads, the hook will first look for a previously cached value. If one is found, it will be used as the initial value instead of the provided initial value.

```jsx
import { useLocalStorage } from 'crooks'

const App = () => {
  const [state, setState] = useLocalStorage("LOCAL_STORAGE_KEY", initialValue)

  return (
    <div>App</div>
  )
}
```

# useFiler

`useFiler` manages a simple virtual file system using the browser\'s localstorage. This is especially useful for quick prototyping. Any type of data can be saved in a file provided that it's JSON-serializable.

### Basic Usage

When the hook is first initialized it returns the files as an empty object.

```jsx
import { useFiler } from 'crooks'

const App = () => {
  const [files, {add, remove, update, clear}] = useFiler("LOCAL_STORAGE_KEY")

  return (
    <div>App</div>
  )
}
```

#### File Structure

By default, each file has an automatically generated id generated using the [shortid](https://www.npmjs.com/package/shortid) package. Each single file is structured as follows:
```
{
  id: "ogn41na",
  created: 489108491,
  modified: 489108561,
  data: "The file's data."
}
```

The `files` object returned as the first parameter of the hook represents all of the current files as an object, with each file's ID as the key, and the file as the value.

#### Adding Files

The first parameter of the add function may be any JSON-serializable data and is required. The data will be saved as a new file with an automatically generated ID. If you would like to override the automatically-generated ID, you may pass a String or Int as the second parameter and it will be used as the ID. If the ID already exists, the existing file will be overwritten.

```jsx
add("Any JSON-serializable data to be saved as a new file.")
```

#### Updating Files

To update a file, pass as the first parameter, the ID of the file you want to update. The second parameter is the data you want to overwrite the file with.

```jsx
update("jal31af", "The new data to overwrite the file with.")
```

As with the native `useState`, `update()` accepts a callback function injected with the previous file.

```jsx
update("jal31af", file => ([...file.data, "New item"]))
```

#### Removing Files

The remove function simply accepts a file ID of the file you wish to remove.

```jsx
remove("zoep31a")
```

#### Clearing all Files

```jsx
clear()
```

# useKeyboardShortcut

The `useKeyboardShortcut` hook listens to "keydown" events on the Document, and will call the provided action when the specified Javascript keyCode is detected. The shortcut listener is enabled by default, but can be declaratively disabled by passing `disabled: true` to the hook.

[keyboard.info](https://keycode.info) is a great resource for finding Javascript keyCodes.

#### Basic Usage

```jsx
import { useKeyboardShortcut } from 'crooks'

const App = () => {
  const submit = () => {
    console.log('Submitted')
  }

  const {enable, disable} = useKeyboardShortcut({
    keyCode: 13,
    action: submit,
    disabled: false // This key is not required
  })

  return (
    <div>
      <button onClick={enable}>Enable</button>
      <button onClick={disable}>Disable</button>
    </div>
  )
}
```

With keyboard shortcuts, there are times when you may want to imperatively enable or disable the shortcut listener. For these occasions, the hook returns `enable` and `disable` functions.

# useOnClickOutside

`useOnClickOutside` accepts a function that will be called when there's a click outside of a target element. The hook returns a ref, which you pass to the ref attribute of the element you want to target.

#### Basic Usage

```jsx
import { useOnClickOutside } from 'crooks'

const App = () => {
  const handleClickOutside = () => {
    console.log("You clicked outside of the blue box")
  }

  const outsideRef = useOnClickOutside(handleClickOutside)

  return (
    <div>
      <div ref={outsideRef}> I'm a blue box </div>
    </div>
  )
}
```

#### Disabling the listener

For performance reasons, you may not want to always listen for clicks outside of an element. For these times you can pass a `Boolean` as a second parameter to this hook representing whether or not the listener should be disabled like so:


```jsx
import { useState } from 'react'
import { useOnClickOutside } from 'crooks'

const App = () => {
  const [isDisabled, setIsDisabled] = useState(false)

  const disableOnOutside = () => setIsDisabled(true)

  const handleClickOutside = () => {
    console.log("You clicked outside of the blue box")
  }

  const outsideRef = useOnClickOutside(handleClickOutside, isDisabled)

  return (
    <div>
      <button onClick={disableOnOutside}>Stop listening for outside clicks</button>
      <div ref={outsideRef}> I'm a blue box </div>
    </div>
  )
}
```

# imageSize

## API

### Params

```js
/**
 * Params
 * @param {string} url - The image url
 */
```

### Returns

```js
/**
 * Returns
 * @param {array} size - The image size [width, height]
 */
```

## Usage

```js
import React from 'react';

import useImageSize from '@use-hooks/image-size';

export default function App() {
  const url = 'https://cdn.int64ago.org/ogk39i54.png';
  const [width, height] = useImageSize(url);

  return (
    <div>
      <h2>DEMO of <span style={{ color: '#F44336' }}>@use-hooks/image-size</span></h2>
      <div>
        <img src={url} width={100} height={100} alt="" />
        <div>Natural size: {width} x {height}</div>
      </div>
    </div>
  );
}

```

# usePermissions

## Usage

```js
import usePermissions from '../src';

const format = hasPermissions => {
  switch (hasPermissions) {
    // User has granted permissions
    case true: {
      return "Permissions granted";
    }
    // User has denied permissions
    case false: {
      return "Permissions denied";
    }
    // User will be prompted for permissions
    case null: {
      return "Asking for permissions";
    }
  }
}

function App() {
  const hasPermissions = usePermissions("geolocation");
  const content = format(hasPermissions);
  return <h1>{content}</h1>;
}
```

# useClientHydrated

## API

```js
const output = useClientHydrated()
```

## Example

```jsx
import React, { useEffect, useState } from 'react'
import useClientHydrated from '@charlietango/use-client-hydrated'

const Component = () => {
  const hydrated = useClientHydrated()

  // Set the initial ready state based on hydrated. Will be `false` first time the component is rendered, but true after hydration.
  const [ready, setReady] = useState(hydrated)

  useEffect(() => {
    // We have been hydrated correctly now
    setReady(true)
  }, [])

  return <div>{ready ? 'Hydrated' : 'Hydrating'}</div>
}

export default Component
```

# useElementSize

## API

```js
const [ref, size] = useElementSize()
```

The hook returns an Array with a `ref` function, and the measured `size`.
Assign the `ref` to the element you want to measure.

## Example

```jsx
import React from 'react'
import useElementSize from '@charlietango/use-element-size'

const Component = () => {
  const [ref, size] = useElementSize()
  return (
    <div ref={ref}>
      <pre>
        <code>{JSON.stringify(size, null, 2)}</code>
      </pre>
    </div>
  )
}

export default Component
```

# useFocusTrap

## API

```js
const ref = useFocusTrap(active, options)
```

The `useFocusTrap` hook returns a `ref` that you should assign to the DOM element that needs to trap focus.
Anything outside that element, will no longer be able to receive focus.

You can toggle the trap by setting the `active` boolean. By default it's activated once the `ref` is assigned.

### Options

The focus trap accepts an object with these optional options, that give you a bit more control.

| Name                 | Type                     | Default     | Required | Description                                                                                                                                                       |
| -------------------- | ------------------------ | ----------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **focusSelector**    | `string` / `HTMLElement` | `undefined` | false    | Assign focus when activated to the element matching this selector - or a specific element you supply. By default focus will be set to the first valid tab target. |
| **disableAriaHider** | `boolean`                | `false`     | false    | Disables setting `aria-hidden` on other elements inside the `document.body` while the trap is active.                                                             |

## Example

```jsx
import React from 'react'
import useFocusTrap from '@charlietango/use-focus-trap'

const Component = () => {
  const ref = useFocusTrap()
  return (
    <div ref={ref}>
      <button>Trapped to the button</button>
    </div>
  )
}

export default Component
```

### Creating a Modal

When using this hook to create a Modal, there are still a few things you need to handle, that's outside the scope of this hook:

- Trap focus
- Close on escape
- Close on backdrop clicked

#### BaseModal.tsx

This is the base component for creating a `<Modal />`. It receives an `onRequestClose` method,
that can be triggered to tell the containing component to update it's state to close the modal.
It doesn't contain a `<Backdrop />`, but that would be a absolute positioned component, that
when clicked triggers the `onRequestClose` method.

```typescript jsx
import React, { useEffect } from 'react'
import ReactDOM from 'react-dom'
import useFocusTrap from '@charlietango/use-focus-trap'
import styled from 'styled-components'

type Props = {
  onRequestClose?: () => void
  children?: React.ReactNode
  isOpen: boolean
  className?: string
}

const Wrapper = styled.div`
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  z-index: 5;
`

function BaseModal({ children, isOpen, onRequestClose, className }: Props) {
  const ref = useFocusTrap()

  function handleKeyDown(event: KeyboardEvent) {
    if (event.key === 'Escape') {
      if (onRequestClose) onRequestClose()
    }
  }

  useEffect(() => {
    if (isOpen) {
      document.addEventListener('keydown', handleKeyDown)
      return () => {
        document.removeEventListener('keydown', handleKeyDown)
      }
    }

    return
  }, [isOpen])

  const modal = (
    <Wrapper
      ref={ref}
      style={{ pointerEvents: !isOpen ? 'none' : undefined }}
      role="dialog"
      className={className}
    >
      {children}
    </Wrapper>
  )

  return ReactDOM.createPortal(modal, window.document.body)
}

BaseModal.displayName = 'BaseModal'
BaseModal.defaultProps = {}

export default BaseModal
```

# useId

## API

```js
const id = useId(prefix?: string)
```
-----

## The Hooks

https://github.com/charlie-tango

### Individual hooks

All of our Hooks are published into their own NPM module, so you can pick and choose exactly the ones you need.

<!-- HOOKS_START -->

- **[@charlietango/use-client-hydrated](https://www.npmjs.com/package/@charlietango/use-client-hydrated)** _([useClientHydrated](packages/useClientHydrated/src))_ - Check if the client has been hydrated
- **[@charlietango/use-element-size](https://www.npmjs.com/package/@charlietango/use-element-size)** _([useElementSize](packages/useElementSize/src))_ - Measure the size of a DOM element using ResizeObserver
- **[@charlietango/use-focus-trap](https://www.npmjs.com/package/@charlietango/use-focus-trap)** _([useFocusTrap](packages/useFocusTrap/src))_ - Trap keyboard focus inside a DOM element, to prevent the user navigating outside a modal
- **[@charlietango/use-id](https://www.npmjs.com/package/@charlietango/use-id)** _([useId](packages/useId/src))_ - Generate a deterministic id using a Context Provider
- **[@charlietango/use-interaction](https://www.npmjs.com/package/@charlietango/use-interaction)** _([useInteraction](packages/useInteraction/src))_ - Monitor the user interactions on an element
- **[@charlietango/use-lazy-ref](https://www.npmjs.com/package/@charlietango/use-lazy-ref)** _([useLazyRef](packages/useLazyRef/src))_ - Create a new ref with lazy instantiated value
- **[@charlietango/use-media](https://www.npmjs.com/package/@charlietango/use-media)** _([useMedia](packages/useMedia/src))_ - Detect if the browser matches a media query
- **[@charlietango/use-native-lazy-loading](https://www.npmjs.com/package/@charlietango/use-native-lazy-loading)** _([useNativeLazyLoading](packages/useNativeLazyLoading/src))_ - Detect if the browser supports the new 'loading' attribute on Image elements.
- **[@charlietango/use-script](https://www.npmjs.com/package/@charlietango/use-script)** _([useScript](packages/useScript/src))_ - Load an external third party script
- **[@charlietango/use-toggle](https://www.npmjs.com/package/@charlietango/use-toggle)** _([useToggle](packages/useToggle/src))_ - Simple boolean state toggler
- **[@charlietango/use-window-size](https://www.npmjs.com/package/@charlietango/use-window-size)** _([useWindowSize](packages/useWindowSize/src))_ - Get the width and height of the viewport

<!-- HOOKS_END -->

### `@charlietango/hooks`

The [@charlietango/hooks](https://www.npmjs.com/package/@charlietango/hooks)
module collects all of the individual modules into a single dependency. The module
is optimized for tree shaking, so you application should only include the dependencies
you actually use.

-----
