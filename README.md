# Commit Version and Create Release

GitHub action for committing and pushing changes related to versioning an for creating a new release with artefacts.

Uses tools [newchanges] and [vp]. Usually run after [prantlf/bump-version-action]. Only platforms Linux, macOS, Windows on the architecture X64 are supported.

## Usage

Commit and push the changelog using the latest version written to it and create a new release:

```yml
- uses: prantlf/finish-release-action@v1
  with:
    files: CHANGELOG.md
```

Work only in specific release branches, run only if new version is detected in the commit messages by [prantlf/bump-version-action]:

```yml
jobs:
  build:
    steps:
    - uses: actions/checkout@v4
    - uses: prantlf/setup-v-action@v2
    - name: Bump version
      id: bump
      uses: prantlf/bump-version-action@v1
    - run: ...
    - uses prantlf/finish-release-action@v1
      if: ${{ steps.bump.outputs.bumped }}
      with:
        branches: master v1.x
```

## Inputs

The following parameters can be specified using the `with` object:

### branches

Type: `String`<br>
Default: `'main master'`

Branches which this action should run for, which are used to publishing releases. Use whitespace for separating the branch names. If you want to use multiple lines in YAML, introduce them with ">-". If you want to allow all branches, set the value to "*".

### enable

Type: `Boolean`<br>
Default: `true`

Can be set to `false` to prevent this action from running. It's helpful in the pipeline, which will not continue releasing, but only building and testing, and that will be decided in the middle of a job execution.

### no-bump

Type: `Boolean`<br>
Default: `false`

Set to `true` not to bump the version number in source files. Only the changelog will be modified.

### no-archives

Type: `Boolean`<br>
Default: `false`

Set to `true` not to upload platform archives automatically as release assets.

### dry-run

Type: `Boolean`<br>
Default: `false`

Can be set to `true` to only print what would be done without actually doing it.

### debug

Type: `Boolean`<br>
Default: `false`

Can be set to `true` to enable debug logging of the supporting tools. Debug logging will be enabled also if it's enabled on the runner.

## License

Copyright (C) 2023 Ferdinand Prantl

Licensed under the [MIT License].

[MIT License]: http://en.wikipedia.org/wiki/MIT_License
[prantlf/bump-version-action]: https://github.com/prantlf/prantlf/bump-version-action
[newchanges]: https://github.com/prantlf/v-newchanges
[vp]: https://github.com/prantlf/vp
