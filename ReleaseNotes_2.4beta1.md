# Baseline v.2.4beta1 Release Notes
Huge thanks to all of the contributors and idea generators from the community, you all make this tool way better than it would be.

Remember that during betas, you can point iMazing Profile Editor at the `ProfileManifest` folder to get the new keys before they're released to the public profile manifest library.

## Please Help Test Betas
If you rely on Baseline for deployments and updates, we would really appreciate testing your existing configurations against the new beta releases.

If you're looking to utilize these new features, please test them out and let us know if any issues arise.

The #baseline channel on the [Mac Admins Slack](https://macadmins.org) is the best place to get support or give feedback.

## New Features
- Added the ability to dynamically hide or show the DIalogListView while processing items.
    - Use `HideListView` boolean for any item in `Scripts` `Installomator` or `Packages` to hide the list view while that item runs
    - Note: Some versions of SwiftDialog appear to have bugs related to this feature along with `--blurscreen`.
- Added the ability to use custom Status icons for all Dialog List View item types.
    - By default, Baseline will continue to use the default swiftDialog `wait` `success` `fail` status types for line items.
    - Admins can choose a Global Default Icon for each of these three statuses which all item types will use:
        - `GlobalDialogStatusIconWait`
        - `GlobalDialogStatusIconSuccess`
        - `GlobalDialogStatusIconFail`
    - There is also a new default icon option for Pending `WaitFor` items:
        - `PendingWaitForIcon`
        - All WaitFor items will get this status applied at the start of the Dialog List View
    - This feature requires SwiftDialog 3.0, which is pending official release.
- Added the ability to use custom Status icons for each Dialog List View item type
    - The following keys can now by added to any item in the `Installomator` `Scripts` or `Packages` categories:
        - `StatusIconWait`
        - `StatusIconSuccess`
        - `StatusIconFail`
    - Any item without one of these keys will use the default for that status.
    - This feature requires SwiftDialog 3.0, which is pending official release.

## Improvements
- Cleaned up code for sending status updates

## Bug Fixes
- Fixed an issue with Dialog options handling where using `--messagefont` would string match with `--message` causing the default Baseline messaging to be lost.
    - Huge thanks to @george-elphick-talieisin #103
- Fixed an issue with `WaitFor` items throwing off the progress bar.

## Known Issue:

