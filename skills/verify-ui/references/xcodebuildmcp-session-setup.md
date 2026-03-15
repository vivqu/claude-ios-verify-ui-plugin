# XcodeBuildMCP Session Setup

**Skip this entire section if `session_show_defaults` already shows project/workspace, scheme, and simulator all set.**

Otherwise, run the following steps using `XcodeBuildMCP` tools:

1. Determine the minimum simulator version to use:
   - Call `show_build_settings` and look for `IPHONEOS_DEPLOYMENT_TARGET`. If found, use that version as the minimum (e.g. `16.0` → require iOS 16+).
   - If `show_build_settings` is unavailable or `IPHONEOS_DEPLOYMENT_TARGET` is not set, default to iOS 18+.
2. Call `list_sims` and confirm at least one iOS simulator exists at or above the minimum version. If none, stop and tell the user to install one in Xcode.
3. From the matching results, find any with state **Booted**. If exactly one is booted, call `session_set_defaults` to use it. If multiple are booted, show the list and ask the user to pick one, then call `session_set_defaults`. If none are booted, continue — the build tool will boot one.
4. Call `session_show_defaults`. If `projectPath` or `workspacePath` is missing, call `discover_projs` using the workspace root. If exactly one project or workspace is found, call `session_set_defaults` to set it. If multiple are found, show the list and ask the user to pick one. If none are found, stop and tell the user no Xcode project was found.
5. If `scheme` is still missing after setting the project, call `list_schemes` and set the first result via `session_set_defaults`. If multiple schemes exist, show the list and ask the user to pick one.