---
name: nomac
description: >
  Use a real cloud Mac with Xcode through the NoMac MCP server: start a macOS
  VM, build and test iOS and macOS apps, use the iOS Simulator, move files,
  hand a build to a person, ship to TestFlight and stop the Mac. Use it when a
  task needs macOS or Xcode and this machine is not a Mac.
---

# NoMac: a Mac for your agent

NoMac gives you a full macOS VM with Xcode and administrator access, billed
from the user's prepaid credit at 80 US cents an hour, by the minute. Every Mac
is deleted when it stops.

## Workflow

1. **Credit.** `get_mac_credits` shows the balance and how long a Mac can run
   on it. Too little: `get_mac_checkout` returns a payment link for the human.
   Never enter payment details yourself; wait until the balance appears.
2. **Start.** `start_mac` with a stable `request_key`, then poll
   `get_mac_session` until `ready` (usually under a minute). Its `image` field
   lists macOS, Xcode, Simulator runtimes and the installed tools (Ruby with
   CocoaPods and Bundler, xcbeautify, xcodegen, gh, Git LFS, jq).
3. **Code.** Clone with `exec_mac` (`git clone ...`), write files with
   `write_mac_file`, download with `fetch_to_mac`, or give the human
   `create_mac_upload_link` for a file of up to 2 GB.
4. **Run.** `exec_mac` with an argv, for example
   `["/bin/bash", "-lc", "xcodebuild -scheme App -destination 'platform=iOS Simulator,name=iPhone 17' build | xcbeautify"]`.
   A command that finishes within `wait_seconds` returns its output in the
   same call; set `wait_seconds` to 50 for builds and tests. A longer one
   returns a job ID: poll `get_mac_job`, then `read_mac_output`. Never rerun a
   command to get its output.
5. **Look.** `screenshot_mac` shows you the Mac's screen as an image
   (1440x900; image pixels are screen coordinates), or with
   `target: "simulator"` the booted iOS Simulator. Commands may drive any app
   with AppleScript and System Events without a permission prompt. To show
   the human a video, call `start_mac_recording`, do the work, then
   `stop_mac_recording`, which returns a download link. `open_mac_desktop`
   gives the human the Mac's screen in their browser.
6. **Results.** `read_mac_file` for small files; `create_mac_download_link`
   for build products and folders (zipped), which keep working after the Mac
   stops.
7. **Stop.** `stop_mac` when done, then poll `get_mac_session` until
   `cleanup_confirmed`. Stopping deletes the Mac and its files.

## Rules

- Keep the same `request_key` and arguments when retrying after an uncertain
  response; inspect the existing session or job instead of starting another.
- One Mac at a time per account. Ready time is billed, including idle time.
- App Store Connect tools (`get_testflight`, `manage_testflight`, `publish`)
  need the user's Apple connection at https://nomac.app.
- Full reference for agents: https://nomac.app/llms.txt
- Problems: `report_issue` with the session ID and what went wrong.
