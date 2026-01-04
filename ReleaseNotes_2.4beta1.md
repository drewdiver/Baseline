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
- Added support to retry failed items
    - Default retry value for all types is 0
    - You can set defaults for the three major types using:
        - DefaultScriptRetries
        - DefaultInstallomatorRetries
        - DefaultPackageRetries
    - You can override the deafult retries value for any individual item using: `Retries` on that item's definition
    - Note that Baseline retries do not attempt to redownload remote packages or scripts and do not retry SHA256/MD5 checksum checks. 
        - For download retries, see the new `curlOptions` feature
- Added the ability to dynamically hide or show the DialogListView while processing items.
    - Use `HideListView` boolean for any item in `Scripts` `Installomator` or `Packages` to hide the list view while that item runs.
    - Note: Some versions of SwiftDialog appear to have bugs related to this feature along with `--blurscreen`.
- Added the ability to skip the final Success or Fail windows entirely.
    - `SkipSuccessDialog` is a new top level `boolean` key that defaults to false.
    - `SkipFailDialog` is a new top level `boolean` key that defaults to false.
- New key `ShowList` - Boolean defaults to true
    - If false, the List View dialog will not actually show a list of items. Can be used to create alternate onboarding experiences, like showing a video or image carousel while items are processed.


## Improvements
- Cleaned up code for sending status updates.
- `--version` argument now only outputs the script version string.
- Dialog version now printed to the log and report.
- `--configuration=/path/to/config.plist` is now a supported format for passing configuration file paths at runtime, which should be a nice change for Jamf admins using parameters.
- New default icon for all windows.
    - Apple changed the "Keyboard Assistant" icon Baseline used by default, changing the Tuxedo/Butler to a green checkmark.
    - Baseline will now use the swiftDialog built-in of `computer` which shows an icon in the style of the computer it's running on.
- New command line option: `-k|--keep|--keeptemp`
    - If this option is used at runtime, Baseline will not delete the temp directory.
    - This option is intended only for debugging during testing/development, and will not be added to the configuration profile.
- Temp files inside the temp directory no longer have randomly generated filenames, since the directory path is already randomly generated.

## Bug Fixes
- Fixed an issue with Dialog options handling where using `--messagefont` would string match with `--message` causing the default Baseline messaging to be lost.
    - Huge thanks to @george-elphick-talieisin #103
- Fixed an issue with `WaitFor` items throwing off the progress bar.
- BailOut and ExitCondition checks now happen at more appropriate times.
- Remote hosted Packages that don't end with `.pkg` will now have the file extension applied automatically
    - Thank you @one8three

## Known Issues:
- Inspect Mode JSON can only be a local file path at this time, will need to support remote files, raw strings, and encoded strings.
- The following items are not yet in the Profile Manifest:
    - DefaultScriptRetries
    - DefaultInstallomatorRetries
    - DefaultPackageRetries