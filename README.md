# Dep-watch

`dep-watch` is a tool for detecting changes to dependencies so you know whether anything needs to be restored. This is useful for workflows that involve frequent merging of other branches. Rather than always running `npm i` or `pod install` after every merge to be certain that you have the latest dependencies installed, just run dep-watch to know what to restore.

This is done by storing a snapshot of the dependencies, and comparing to the snapshot when ran. Default snapshot file is `./depwatch.cache.json`. You will likely want to include the snapshot file in your `.gitignore` file. If you are using the default file name you can do this automatically by running `depwatch gitignore` from the project root.

## Motivation

Most of my work is done in `react-native`. I am frequently changing branches, merging changes, etc. As part of my workflow I have become accustomed to running `npm ci && (cd ios && pod install)` frequently to ensure that all dependencies are correct before running the application. For large projects with multiple pre- and post-install scripts this often takes 2-5 minutes to run to completion. In order to save some time, I decided to build a tool that would detect if any changes were made to dependencies, so I could have confidence skipping this step without having to diff the dependency manifests manually.

## Disclaimer

This library is currently being built for my specific use cases. At this time I will not be implementing requested features that are not in the `Future Work` section. If you have a feature that you would like to include, please contact me or put in a pull request.

## Installation

`npm install -g @lyv/depwatch`

## Usage

`depwatch [command] [options]`

## CLI Commands

### Check

Performs comparison of dependencies with the snapshot.

### Gitignore

Appends an entry in the `.gitignore` file to prevent the default snapshot file from being commited.

### Update

Updates the snapshot without running check. Recommended to add this to `postinstall` script to ensure that it is kept in sync.

## CLI options

| Option   | Alias | Description                                                  |
|----------|-------|--------------------------------------------------------------|
| all      | -a    | Checks for changes to all dependencies                       |
| restore  | -r    | Restore dependencies if missing                              |
| update   | -u    | Update the snapshot                                          |
| node     | -n    | Check for changes to JavaScript dependencies                 |
| pods     | -n    | Check for changes to CocoaPod dependencies (not implemented) |

## Package.json

Alternatively, you can specify configuration in the `package.json` file.

```json
{
    "depwatch": {
        "cacheFile": "./cache/depwatch.cache.json", // path to dependency cache file
        "checkNode": true,                          // Whether to check JavaScript dependencies
        "restore": true                             // Whether to restore missing dependencies
    }
}
```

## Future work

- [ ] support for CocoaPods
- [ ] ability to restore packages automatically from `dep-watch`
- [ ] option to only restore dependencies that have changed
- [ ] option to store configuration in `.depwatchrc` instead of just `package.json`
- [ ] add watcher
