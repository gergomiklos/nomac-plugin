---
name: nomac
description: >
  Use a cloud Mac with Xcode and a cloud iPhone through the NoMac MCP server:
  start a macOS VM, build and test iOS and macOS apps, run an app on a cloud
  iPhone Simulator, move files, hand a build to a person, and stop. Use it when
  a task needs macOS, Xcode or an iPhone and this machine is not a Mac.
---

# NoMac: a Mac and an iPhone for your agent

NoMac gives you a full macOS VM with Xcode and administrator access, and a
cloud iPhone Simulator, billed per minute from the user's prepaid credit. The
VM is deleted when it stops; the account's drive at `~/drive` (20 GB) is on
every Mac and kept between sessions.

## The Mac

1. **Credit.** `get_mac_credits` shows the balance and how long a Mac can run
   on it. Too little: `get_mac_checkout` returns a payment link for the human.
   Never enter payment details yourself; wait until the balance appears. Under
   an hour left, answers say `low_credit`: tell the human before a long build.
2. **Start.** `start_mac` with a stable `request_key`, then call
   `get_mac_session` with `wait_seconds` until `ready` (usually under a
   minute). Its `image` field lists macOS, Xcode, Simulator runtimes and the
   installed tools (Ruby with CocoaPods and Bundler, xcbeautify, xcodegen, gh,
   Git LFS, jq).
3. **Code.** Clone into the drive with `exec_mac` (`git clone ... ~/drive/src/<repo>`),
   so the next session rebuilds only what changed. Write files with
   `write_mac_file` (up to 3 MiB). For a bigger file, `create_mac_link` with
   `direction: "upload"` gives a link for up to 2 GB, and `get_mac_file_link`
   moves it onto the Mac once it arrives.
4. **Run.** `exec_mac` with an argv, for example
   `["/bin/bash", "-lc", "cd ~/drive/src/App && xcodebuild -scheme App -destination 'platform=iOS Simulator,name=iPhone 17' build | xcbeautify"]`.
   A quick command returns its output in the same call; set `wait_seconds` to
   40 for builds. One still running is a job that `get_mac_job` follows to its
   end. Never rerun a command to get its output.
5. **Look.** `screenshot_mac` shows the Mac's screen, or with
   `target: "simulator"` the booted iOS Simulator; follow its `next_step`.
   `mac_input` types and clicks as a person would. `mac_recording` (action
   `start`, then `stop`) makes a video for the human; `open_mac_desktop` gives
   them the Mac's screen in their browser.
6. **Results.** `read_mac_file` for small files; `create_mac_link` with
   `direction: "download"` for build products and folders (zipped), which
   keep working after the Mac stops.
7. **Stop.** `stop_mac` when done. Stopping deletes the Mac and every file
   outside `~/drive`. Its answer asks for feedback: if anything got in your way
   or was missing, send a sentence or two with `report_issue` and
   `kind: "feedback"`.

## The iPhone

`phone` with action `start` gives a cloud iPhone in seconds, with no shell;
its answer has `links.view` for the human to watch. `phone_app_run` with `git`
(an https repository link; `path` for a subfolder) or `upload`
(`phone_upload_link` sends a local folder) builds and runs the app,
incrementally between phones. `phone_ui_look` reads the screen as text and
`phone_ui_act` taps and types by label; `phone_app_logs` and `phone_tests_run`
for the rest. A build past 40 s answers with a job for `phone_job_status`.
`phone` with action `stop` when done; it stops after 10 idle minutes.

## Shipping

Signing, TestFlight and the App Store are on the NoMac App Store server,
`https://mcp.nomac.app/appstore`, on the same account and Mac: there
`set_up_signing`, `send_to_testflight`, `publish` (after the human approves)
and `notarize`. Never ask for a secret in the chat: `request_secret` gives the
human a link, and every Mac receives the account's secrets.

## Rules

- Keep the same `request_key` and arguments when retrying after an uncertain
  response; inspect the existing session or job instead of starting another.
- One Mac at a time per account; `start_mac` with `parallel: true` starts a
  second at once, without the drive. Ready time is billed, including idle time.
- Store answers that are legal attestations (age rating, encryption, privacy)
  come from the human: never guess them.
- Full reference for agents: https://nomac.app/llms.txt
- Problems: `report_issue` with the session ID and what went wrong.
