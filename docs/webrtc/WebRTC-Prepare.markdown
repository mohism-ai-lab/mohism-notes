
# 0. Introduction

# 1. Source Code Fetch

## 1.1 Windows platform

### 1.1.1 Prerequisite

1. Python 2.17.
2. GIT Windows version.
3. depot_tools.

Python and git tool's installation is very simple, let's focus on how to install depot_tools which is the special tool to manage chromium's source code.

#### Install depot_tools

Download the [depot_tools](https://storage.googleapis.com/chrome-infra/depot_tools.zip) bundle and extract it somewhere. Then add depot_tools to the start of your PATH (must be ahead of any installs of Python).

Also, add a DEPOT_TOOLS_WIN_TOOLCHAIN system variable in the same way, and set it to 0. This tells depot_tools to use your locally installed version of Visual Studio (by default, depot_tools will try to use a google-internal version).

From a cmd.exe shell, run the command gclient (without arguments). On first run, gclient will install all the Windows-specific bits needed to work with the code, including msysgit and python.

#### Source code download

Then choose the place you want to store the webrtc source code, cd to that directory, run:

```bash
fetch --nohooks webrtc
gclient sync
```

### 1.1.2 Build isssues

> ERROR Need exactly one build directory to generate

When I run the gn command like below:

```bash
gn gen out/debug_clang_vs2019 --args='target_cpu=x64 is_debug=false is_component_build=false is_clang=false rtc_include_tests=false rtc_use_h264=true use_rtti=true use_custom_libcxx=false treat_warnings_as_errors=false use_ozone=true' --ide=vs2019
```

I received the error above. After some investigations, I changed the command like this:

```bash
>gn gen --ide=vs2019 out/release --args="target_cpu=\"x64\" is_debug=false is_component_build=false is_clang=false rtc_include_tests=false rtc_use_h264=true use_rtti=true use_custom_libcxx=false treat_warnings_as_errors=false use_ozone=true"
```
The problem was solved.

### 1.1.3 Build instructions

```bash
gn gen --ide=vs2019 out/release --args="target_cpu=\"x64\" is_debug=false is_component_build=false is_clang=false rtc_include_tests=false rtc_use_h264=true use_rtti=true use_custom_libcxx=false treat_warnings_as_errors=false use_ozone=true"
```

## 1.2 How to view RTC logs on Windows platform

Originally I have no idea about this issue. Then after my tries, I found that it's easy to fix.

1. Adding the build arg "is_debug=true" to the build args' list. then build the whole project.
2. Then install the DebugView on Windows 10, then when you launch the peerconnection_client.exe, you can see the logs were dumped by DebugView.


# 2. PeerConection source code review

# 3. How to enable the high profile levels of H264 decoding and encoding.

## 3.1 High profile level of H264 encoding.

## 3.2 High profile level of H264 decoding.

### References

https://topic.alibabacloud.com/a/let-webrtc-support-h264-codec_8_8_10275189.html


# Control bandwidth

# The practise of Video quality improvement in WebRTC

## About openh264 libray

>Q. Which profiles of H.264 will be supported?
>
>A: The initial code has the baseline profile. We look forward to working with the open source community to add high profile and others.

# Other Tips

In order to run the javascript locally, I have to configure the firefox:

```
privacy.file_unique_origin = false

```

### Reference

https://github.com/berinhard/pyp5js/issues/72

https://groups.google.com/g/discuss-webrtc/c/rMk8tfuVzuE/m/E9cLYfclAgAJ

https://dev.to/dengel29/loading-local-files-in-firefox-and-chrome-m9f