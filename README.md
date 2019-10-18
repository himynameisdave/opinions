<div align="center" margin="0 auto 20px">
    <h1>opinions</h1>
    <p style="font-style: italic;">*️⃣ An ever-evolving repository for documenting my thoughts/opinions about coding-best practices and programming in general.</p>
</div>

---

Programming is highly subjective. Throughout one's career, one will be exposed to many different patterns, concepts and ideas. Some of these ideas are great and they stick around, whereas others might not hold up as well over time. It's important to have well-informed opinions, including the evidence and experience based reasoning for why you hold those beliefs.

It's _more_ important, however, to not be too attached to any particular philosophy or pattern, or to allow emotion cloud your judgement over such concepts. In my opinion, the best developers are those who allow themselves to perpetually grow and learn when exposed to new ideas, and not hold onto outdated or overly niché concepts.

I wanted to document my thoughts and opinions on building software, which I have gathered from developing (primarily web) apps for around a decade. These are by no means hard-and-fast rules to live by. Given that this is a Git repository, I hope that this document grows and evolves with me in my career. I hope to include valuable insights into the reasons I have made these decisions for others to learn and be inspired by.

## Table of Contents

1. [General code style](#General-code-style)
2. [File naming](#File-naming)
3. [Git](#Git)
4. [React](#React)
5. [Redux](#Redux)
999. [Acknowledgments](#Acknowledgments)

## General code Style

### Trailing commas

Always include trailing/"dangling" commas after the last value in an object.

**Example:**

```js
//  Bad
const foo = {
    bar: 'hello',
    baz: 'world'
};

//  Good
const foo = {
    bar: 'hello',
    baz: 'world',
};
```

**Reasoning:**
- Easier code reviews: When reviewing a git diff where a new item was added at the end of an object, comma dangle prevents it showing up as 2 lines changed.


> ESLint: this can be enforced via [this ESLint rule](https://eslint.org/docs/rules/comma-dangle).

## File naming


### Prefer `.jsx`/`.tsx` extension for React files

When working in a React project, I prefer to use the `.jsx` (or `.tsx`) file extension for files which contain React components/JSX.

**Reasoning:**
- In large codebases, it makes it more obvious that you are editing a React component and not just some other code.
- Editors such as VSCode can be configured to display a different icon for `.jsx` files, again making it easier to identify view-layer code.

> ESLint: This can be enforced via [this ESLint rule](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md).


### Prefer kebab-case

When naming files, use [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles). Use only lowercase letters.

**Reasoning:**
- Different OSs treat uppercase letters inconsistently.
- Underscores require you to hold the SHIFT key, making them take longer to type.
- Uppercase letters also require a SHIFT key hold, making them take longer to type.

> ESLint: This can be enforced via [this ESLint rule](https://github.com/selaux/eslint-plugin-filenames#consistent-filenames-via-regex-match-regex).


### No `index` files

Avoid naming files `index.js`.

**Reasoning:**
- Having a bunch of files named `index` makes it harder to distinguish them and open files by name (like via `CMD-P` in most editors).
- Slows down Node's resolver:
    ```js
    // Bad: Resolver has to check foo.js before checking for foo/index.js
    require('./foo');

    require('./foo/index.js'); // Good
    require('./foo/bar.js'); // Better
    ```

> ESLint: This can be enforced via [this ESLint rule](https://github.com/selaux/eslint-plugin-filenames#dont-allow-indexjs-files-no-index).


### Include file extensions in imports

Always include the file extension when `import`ing or `require`ing a module.

**Reasoning:**
- Everyone knows exactly what kind of file is being imported (JS vs JSX vs styles vs assets).
- This _can_ help speed up Node's file resolver (as it knows exactly what the file is called).

> ESLint: This can be enforced via [this ESLint rule](https://github.com/selaux/eslint-plugin-filenames#dont-allow-indexjs-files-no-index).

## Git

### Emojis

Git commit messages should start with an Emoji, preferably one which is related to your commit. If you want more structure, I suggest [`gitmoji`](https://gitmoji.carloscuesta.me/). On macOS, `CTRL`+`CMD`+`SPACE` will bring up the Emoji picker.

**Reasoning:**
- It's fun.
- Can make code reviews more fun.
- Aside from being fun, it can also be used to help visually group commits when looking at a long list of commits.


### Prefer smaller commits

When committing code, prefer to break larger commits into smaller, related chunks.

**Example:**

```bash
# Bad
git commit -m "🎆 Builds entire feature"

# Good
git commit -m "🏗 Scaffold out UI"
git commit -m "🏬 Connect components to store"
git commit -m "🚀 Hook up store to API"
```

**Reasoning:**
- Makes code reviews easier.
- Makes it easier to revert on a more granular level.


## React

### classnames

Use the [`classnames`](https://github.com/JedWatson/classnames) package to simplify concatenating and conditionally adding `className`s in React.

**Example:**

```jsx
// Bad
<div className={`foo ${bar ? 'bar' : ''}`} />

// Good
<div className={classnames('foo', { 'bar': bar })} />
```

**Reasoning:**
- Consistency
- Cleaner way to handle conditional styles
- Handles null/undefined without literally adding the string `undefined` as a `className`


## Redux

### Use action type constants

Store string literal action types as constants.

**Reasoning:**
- They are referenced in multiple places in the app (action creators, reducers, middleware, etc.).

### Use the actionsMap pattern

Use the actionsMap pattern for creating reducers.

**Example:**

```js
const initialState = {
   count: 0,
};

//  Use the actionTypes constants as dynamic keys
const actionsMap = {
    [actionTypes.INCREMENT]: (state) => ({
        ...state,
        count: state.count + 1,
    }),
    [actionTypes.DECREMENT]: (state) => ({
        ...state,
        count: state.count - 1,
    }),
};

//  This would be broken off into a utility somewhere
const reducer = (state, action) => {
    const reducerFn = actionsMap[action.type];
    if (reducerFn) {
        return reducerFn(state, action);
    }
    return state;
};

```

**Reasoning:**
- Helps break reducer into smaller functions.
- These smaller functions could be unit tested independently.
- Lengthy `switch` or `if`/`else` statements can be unwieldy.

### Use reselect

Use [`reselect`](https://github.com/reduxjs/reselect) library for doing state selection (in [`mapStateToProps`](https://react-redux.js.org/using-react-redux/connect-mapstate) and otherwise).

**Reasoning:**
- Memoization helps make state selection faster.
- Can be composed together to derive complex state.
- Can be tested to ensure components are receiving the correct state.


## Acknowledgments

I'd like to thank everyone I've worked with throughout my career, for without you fine folks I would not be where I am today. I've learned a ton about software development and have been inspired by the folks around me, so thank you all.

---

> _Published by [Dave](http://himynameisdave.com)._
