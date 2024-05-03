# Contributing

## Table of contents

- [Development](#development)
  - [Creating tokens](#creating-tokens)
  - [Publishing package](#publishing-package)
    - [Beta releases](#beta-releases)
- [Token naming conventions](#token-naming-conventions)
- [Scripts](#scripts)

## Development

_Reference [Development](https://github.com/bos-sci/anatomy-docs/blob/develop/CONTRIBUTING.md#development) and [Git Naming](https://github.com/bos-sci/anatomy-docs/blob/develop/CONTRIBUTING.md#git-naming) guidelines for anatomy-docs._

### Creating tokens

1. Reference the latest [primitive](https://www.figma.com/file/9WvScdNUNshlIj7qUS9Nzy/ADS-Primitive-colors?type=design&node-id=2-6&mode=design&t=S4zGdDfDTteWfeS8-0) and [abstract](https://www.figma.com/file/8JFFgisoQRC952AStQf73v/ADS-Foundations-%2F-abstract-colors?type=design&node-id=3541-67&mode=design&t=zmOjDzUGt61iHeXD-0) design tokens in Figma.
2. Create your token in JSON syntax e.g:
   - _See [token naming conventions](#token-naming-conventions) below on how to name your tokens._
   1. Primitive:

```json
"neutral-00": {
  "value": "#000000"
}
```

2.  Abstract:

```json
"border-color-assertive": {
  "value": "{neutral colors.neutral-00}"
}
```

3. Add the new token to theme files:
   1. For primitives:
   - `/src/tokens/<theme-name>/globals/`
   2. For abstracts:
   - `/src/tokens/<theme-name>/light.json`
   - `/src/tokens/<theme-name>/dark.json`
4. _Avoid referencing primitives directly in your code. Use primitive tokens to create abstract tokens._

### Publishing package

1. Pull the latest from `develop` and create a branch using the pattern `release/vX.Y.Z`
2. Run `npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease | from-git]`
3. Create a pull request from `release/vX.Y.Z` into `develop`
4. Create a pull request from `develop` into `main`
5. Publish a release in GitHub
   - This will trigger an npm publish

\* _The repository secret `NPM_TOKEN` is an auth token that allows GitHub to publish. It comes from Ash Johns' npm account and is set to not expire._

#### Beta releases

1. Pull the latest from `develop` and create a branch using the pattern `release/X.Y.Z-beta.A`
2. Run `npm version [beta-version-name]` (write out full version name eg: 5.0.0-beta.9)
3. Create a pull request from `release/X.Y.Z-beta.A` into `develop`
4. Run `npm publish --tag beta`

## Token naming conventions

- All new tokens must be placed in their respective groups eg:

```json
"neutral colors": {
  "neutral-00": {
    "value": "#000000"
    }
  }
```

- If a token requires a new group to be created, specify the type property at group level, e.g:

```json
"borders": {
  "type": "color",
}
```

- Component tokens should be named `component` > `element` > `modifier` > `state`
  - eg: `button-text-color-inverse`

## Scripts

In the project directory, run:

### `npm run build`

This builds the tokens for production in the `lib` folder.

- _Note: This runs on PR._

### `npm run version`

This updates the tokens version number in the README, and stages the README changes in git. This [runs automatically during the `npm version` command](https://docs.npmjs.com/cli/v7/commands/npm-version#description) before the commit and after the package version change. It does not need to be run manually.
