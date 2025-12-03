# Baseline v.2.4beta1 Release Notes
Huge thanks to all of the contributors and idea generators from the community, you all make this tool way better than it would be.

Remember that during betas, you can point iMazing Profile Editor at the `ProfileManifest` folder to get the new keys before they're released to the public profile manifest library.

## Please Help Test Betas
If you rely on Baseline for deployments and updates, we would really appreciate testing your existing configurations against the new beta releases.

If you're looking to utilize these new features, please test them out and let us know if any issues arise.

The #baseline channel on the [Mac Admins Slack](https://macadmins.org) is the best place to get support or give feedback.

## Initial support for swiftDialog 3.0 features - Experimental
These features require swiftDialog 3.0, which is still in testing, and thus features may change. swiftDialog 3.0 also requires macOS 15+.
- Adding support for Inspect Mode
    - New configuration option `InspectModeJSON` lets you provide a file path to a JSON file for use with Inspect Mode.
        - If a value is provided, Baseline will use Inspect Mode instead of the default List View.
    - Documentation for Inspect Mode can be found here: [https://swiftdialog.app/advanced/inspect-mode/](https://swiftdialog.app/advanced/inspect-mode/)
    - When using Inspect Mode, `DialogInspectModeOptions` are used to define some window behavior, however, with Dialog Inspect Mode the admin must provide a JSON file with a valid configuration.
        - Please consult the [swiftDialog documentation](https://swiftdialog.app/advanced/inspect-mode/) for details on how to configure a valid JSON file and choose the desired Preset.
    
- Added the ability to use custom Status icons for all Dialog List View item types.
    - By default, Baseline will continue to use the default swiftDialog `wait` `success` `fail` status types for line items.
    - Admins can choose a Global Default Icon for each of these three statuses which all item types will use:
        - `GlobalDialogStatusIconWait`
        - `GlobalDialogStatusIconSuccess`
        - `GlobalDialogStatusIconFail`
    - There is also a new default icon option for Pending `WaitFor` items:
        - `PendingWaitForIcon`
        - All WaitFor items will get this status applied at the start of the Dialog List View.
- Added the ability to use custom Status icons for each Dialog List View item type.
    - This feature requires SwiftDialog 3.0, which is pending official release.
    - The following keys can now by added to any item in the `Installomator` `Scripts` or `Packages` categories:
        - `StatusIconWait`
        - `StatusIconSuccess`
        - `StatusIconFail`
    - Any item without one of these keys will use the default for that status.

## New Features
- Added the ability to dynamically hide or show the DialogListView while processing items.
    - Use `HideListView` boolean for any item in `Scripts` `Installomator` or `Packages` to hide the list view while that item runs.
    - Note: Some versions of SwiftDialog appear to have bugs related to this feature along with `--blurscreen`.
- Added the ability to skip the final Success or Fail windows entirely.
    - `SkipSuccessDialog` is a new top level `boolean` key that defaults to false.
    - `SkipFailDialog` is a new top level `boolean` key that defaults to false.

## Improvements
- Cleaned up code for sending status updates.
- `--version` argument now only outputs the script version string.
- Dialog version now printed to the log and report.
- `--configuration=/path/to/config.plist` is now a supported format for passing configuration file paths at runtime, which should be a nice change for Jamf admins using parameters.
- New default icon for all windows.
    - Apple changed the "Keyboard Assistant" icon Baseline used by default, changing the Tuxedo/Butler to a green checkmark.
    - Baseline will now use the swiftDialog built-in of `computer` which shows an icon in the style of the computer it's running on.

## Bug Fixes
- Fixed an issue with Dialog options handling where using `--messagefont` would string match with `--message` causing the default Baseline messaging to be lost.
    - Huge thanks to @george-elphick-talieisin #103
- Fixed an issue with `WaitFor` items throwing off the progress bar.
- BailOut and ExitCondition checks now happen at more appropriate times.

## Known Issues:
- Spamming the `show:` command is problematic in Dialog 3.0 (makes some weird screen bounce things happen along the edges), need to rework this to only issue the `show:` command if actually needed.
- Inspect Mode JSON can only be a local file path at this time, will need to support remote files, raw strings, and encoded strings.
