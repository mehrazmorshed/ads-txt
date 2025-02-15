# Contributing and Maintaining

First, thank you for taking the time to contribute!

The following is a set of guidelines for contributors as well as information and instructions around our maintenance process. The two are closely tied together in terms of how we all work together and set expectations, so while you may not need to know everything in here to submit an issue or pull request, it's best to keep them in the same document.

## Ways to contribute

Contributing isn't just writing code - it's anything that improves the project. All contributions for Ads.txt Manager are managed right here on GitHub. Here are some ways you can help:

### Reporting bugs

If you're running into an issue with the plugin, please take a look through [existing issues](https://github.com/10up/ads-txt/issues) and [open a new one](https://github.com/10up/ads-txt/issues/new) if needed. If you're able, include steps to reproduce, environment information, and screenshots/screencasts as relevant.

### Suggesting enhancements

New features and enhancements are also managed via [issues](https://github.com/10up/ads-txt/issues). As project owners, 10up sets the [direction and roadmap](#roadmap) and may not prioritize or decide to implement if outside of the main goals of the plugin.

### Pull requests

Pull requests represent a proposed solution to a specified problem. They should always reference an issue that describes the problem and contains discussion about the problem itself. Discussion on pull requests should be limited to the pull request itself, i.e. code review.

For more on how 10up writes and manages code, check out our [10up Engineering Best Practices](https://10up.github.io/Engineering-Best-Practices/).

### Testing

Helping to test an open source project and provide feedback on success or failure of those tests is also a helpful contribution.  You can find details on the Critical Flows and Test Cases in [this project's GitHub Wiki](https://github.com/10up/ads-txt/wiki) as well as details on our overall approach to [Critical Flows and Test Cases in our Open Source Best Practices](https://10up.github.io/Open-Source-Best-Practices/testing/#critial-flows).  Submitting the results of testing via our Critical Flows as a comment on a Pull Request of a specific feature or as an Issue when testing the entire project is the best approach for providing testing results.

## Maintenance process

### Triage

Issues and WordPress.org forum posts should be reviewed weekly and triaged as necessary. Not all tasks have to be done at once or by the same person. Triage tasks include:

* Responding to new WordPress.org forum posts and GitHub issues/PRs with an acknolwedgment and following up on existing open/unresolved items that have had movement in the previous week.
* Marking forum posts as resolved when corresponding issues are fixed or as not a support issue if not relevant.
* Creating GitHub issues for WordPress.org forum posts as necessary or linking to them from existing related issues.
* Applying labels and milestones to GitHub issues.

#### Issue labels

All issues should be labeled as bugs (`type:bug`), enhancements/feature requests (`type:enhancement`), or questions/support (`type:question`). Each issue should only be of one "type".

Bugs and enhancements that are closed without a related change should be labeled as `declined`, `duplicate`, or `invalid`. Invalid issues would be where a problem is not reproducible or opened in the wrong repo and should be relatively uncommon. These labels are all prefixed with `closed:`.

There are two other labels that are GitHub defaults with more global meaning we've kept: `good first issue` and `help wanted`.

### Review against WordPress updates

During weekly triage, the tested up to version should be compared against the latest version of WordPress. If there's a newer version of WordPress, the plugin should be re-tested using any automated tests as well as any manual tests indicated below, and the tested up to version bumped and committed to both GitHub and the WordPress.org repository.

### Release cycle

New releases are targeted based on number and severity of changes along with human availability. When a release is targeted, a due date will be assigned to the appropriate milestone.

### Testing

Make an ads.txt with the following contents and ensure that you receive the same errors as below. There are no automated tests at this time - contributions very welcome in this area!

#### Contents

```
# This is a comment
contact=test@example.com
subdomain=wrongdomain
subdomain=sub.domain.com

Invalid record

# Records
google.com, pub-1234567890, DIRECT, f08c47fec0942fa0
not-an-exchange, pub-1234567890, INVALID, f08c47fec0942
```

#### Errors

```
Line 3: wrongdomain does not appear to be a valid subdomain
Line 6: Invalid record
Line 10: not-an-exchange does not appear to be a valid exchange domain
Line 10: Third field should be RESELLER or DIRECT
Line 10: f08c47fec0942 does not appear to be a valid TAG-ID
```

### Release instructions

1. Branch: Starting from develop, cut a release branch named `release/X.Y.Z` for your changes.
2. Version bump: Bump the version number in `ads-txt.php`, `package.json`, `package-lock.json` and `readme.txt` if it does not already reflect the version being released. In `ads-txt.php` update both the plugin "Version:" property and the plugin `ADS_TXT_MANAGER_VERSION` constant.
3. Changelog: Add/update the changelog in both `readme.txt` and `CHANGELOG.md`.
4. Credits: Update `CREDITS.md` to include new contributors, manually removing any bots.
5. New files: Check to be sure any new files/paths that are unnecessary in the production version are included in `.gitattributes`.
6. Readme updates: Make any other readme changes as necessary. `CHANGELOG.md` and `README.md` are geared toward GitHub and `readme.txt` contains WordPress.org-specific content. The two are slightly different.
7. Merge: Make a non-fast-forward merge from your release branch to `develop` (or merge the pull request), then do the same for `develop` into `trunk`, ensuring you pull the most recent changes into `develop` first (`git checkout develop && git pull origin develop && git checkout trunk && git merge --no-ff develop`).  `trunk` contains the stable development version.
8. Push: Push your `trunk` branch to GitHub (e.g. `git push origin trunk`).
9. [Compare](https://github.com/10up/ads-txt/compare/trunk...develop) trunk to develop to ensure no additional changes were missed.
10. Test the pre-release ZIP locally by [downloading](https://github.com/10up/ads-txt/actions/workflows/build-release-zip.yml) it from the Build release zip action artifact and installing it locally. Ensure this zip has all the files we expect, that it installs and activates correctly and that all basic functionality is working.
11. Either perform a regression testing utilizing the available [Critical Flows](https://10up.github.io/Open-Source-Best-Practices/testing/#critical-flows) and Test Cases or if [end-to-end tests](https://10up.github.io/Open-Source-Best-Practices/testing/#e2e-testing) cover a significant portion of those Critical Flows then run e2e tests.  Only proceed if everything tests successfully.
12. Release: Create a [new release](https://github.com/10up/ads-txt/releases/new), naming the tag and the release with the new version number, and targeting the `trunk` branch. Paste the changelog from `CHANGELOG.md` into the body of the release and include a link to the closed issues on the `X.Y.Z` milestone (e.g. <https://github.com/10up/ads-txt/milestone/1?closed=1>).
13. SVN: Wait for the [GitHub Action](https://github.com/10up/ads-txt/actions) to finish deploying to the WordPress.org repository. If all goes well, users with SVN commit access for that plugin will receive an emailed diff of changes.
14. Check WordPress.org: Ensure that the changes are live on [WordPress.org](https://wordpress.org/plugins/ads-txt/). This may take a few minutes.
15. Close milestone: Edit the [X.Y.Z milestone](https://github.com/10up/ads-txt/milestone/#) with release date (in the `Due date (optional)` field) and link to GitHub release (in the `Description` field), then close the milestone.
16. Punt incomplete items: If any open issues or PRs which were milestoned for `X.Y.Z` do not make it into the release, update their milestone to `X.Y.Z+1`, `X.Y+1.0`, `X+1.0.0` or `Future Release`.
