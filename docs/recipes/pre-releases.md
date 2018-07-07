# Publishing pre-releases

This recipe will walk you through a simple example that uses pre-releases to publish beta versions while working on a future major release and then make only one release on the default distribution.

This example uses the **semantic-release** default configuration:
- [branches](../usage/configuration.md#branches): `['+([1-9])?(.{+([1-9]),x}).x', 'master', 'next', 'next-major', {name: 'beta', prerelease: true}, {name: 'alpha', prerelease: true}]`
- [plugins](../usage/configuration.md#plugins): `['@semantic-release/commit-analyzer', '@semantic-release/release-notes-generator', '@semantic-release/npm', '@semantic-release/github']`

## Initial release

We'll start by making the first commit of the project, with the code for the initial release and the message `feat: initial commit`. When pushing that commit on `master` **semantic-release** will release the version `1.0.0` and make it available on the default distribution channel which is the dist-tag `@latest` for npm.

The Git history of the repository is:

```
* feat: initial commit # => v1.0.0 on @latest
```

## Working on a future release

We now decide to work on a future major release, which will be composed of multiple features. We want to publish our package for each new feature developed for test purpose, however we do not want to increment our package version or make it available to our users until all the features are developed and tested.

To implement that workflow we can create the branch `beta` and commit our first feature there. When pushing that commit on `beta` **semantic-release** will publish the pre-release version `2.0.0-beta.1` on the dist-tag `@beta`. That allow us to run integration tests by installing our module with `npm install example-module@beta`. Other users installing with `npm install example-module` will still receive the version `1.0.0`.

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
```

We can continue to work on our next release by committing and pushing other features or bug fixes on the `beta` branch. With each push **semantic-release** will publish a new pre-release on the dist-tag `@beta`, which allow us to run our integration tests.

With another feature, the Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
```

## Releasing a bug fix on the default distribution channel

In the meantime we can also continue to commit changes and release updates to our users. For example we can commit a bug fix with the message `fix: a fix` to `master`. When pushing that commit **semantic-release** will release the version `1.0.1` on the dist-tag `@latest`.

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
```

## Working on another future release

TODO

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
```

TODO

With another feature, the Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
|  |  * feat: second feature of other release # => v3.0.0-alpha.2 on @alpha
```

## Publishing the beta release to the default distribution channel

Once we've developed and pushed all the feature we want to include in the future version `2.0.0` in the `beta` branch and all our tests are successful we can release it to our users. To do so we need to merge our changes made on `beta` into `master`. As `beta` and `master` branches have diverged, this merge might require to resolve conflicts.

Once the conflicts are resolved and the merge commit is pushed to `master` **semantic-release** will release the version `2.0.0` on the dist-tag `@latest`.

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
|  |  * feat: second feature of other release # => v3.0.0-alpha.2 on @alpha
| /|  |
*  |  | Merge branch beta into master # => v2.0.0 on @latest
```

## Publishing the alpha release to the default distribution channel

TODO

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
|  |  * feat: second feature of other release # => v3.0.0-alpha.2 on @alpha
| /|  |
*  |  | Merge branch beta into master # => v2.0.0 on @latest
|  | /|
|  |/ |
| /|  |
*  |  | Merge branch alpha into master # => v3.0.0 on @latest
```

## Working on a third future release

TODO

The Git history of the repository is now:

```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
|  |  * feat: second feature of other release # => v3.0.0-alpha.2 on @alpha
| /|  |
*  |  | Merge branch beta into master # => v2.0.0 on @latest
|  | /|
|  |/ |
| /|  |
*  |  | Merge branch alpha into master # => v3.0.0 on @latest
| \|  |
|  *  | Merge branch master into beta
|  *  | feat: new feature # => v3.1.0-beta.1 on @beta
```


TODO change above with this
```
* feat: initial commit # => v1.0.0 on @latest
| \
|  * feat: first feature \n\n BREAKING CHANGE: it breaks something # => v2.0.0-beta.1 on @beta
|  * feat: second feature # => v2.0.0-beta.2 on @beta
*  | fix: a fix # => v1.0.1 on @latest
|  | \
|  |  * feat: first feature of other release \n\n BREAKING CHANGE: it breaks something # => v3.0.0-alpha.1 on @alpha
|  |  * feat: second feature of other release # => v3.0.0-alpha.2 on @alpha
| /|  |
*  |  | Merge branch beta into master # => v2.0.0 on @latest
|  | /|
|  *  | Merge branch alpha into beta # => v3.0.0-beta.1 on @beta
| /|  |
*  |  | Merge branch alpha into master # => v3.0.0 on @latest
| \|  |
|  *  | Merge branch master into beta
|  *  | feat: new feature # => v3.1.0-beta.1 on @beta
```
