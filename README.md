# Introduction

[CrashProbe](http://crashprobe.com/) provides a set of test crashes that can be used to test crash reporting SDKs and symbolication implementations on iOS and OS X.

This repository contains all the raw data that is gathered for each service and platform.

# Data Structure

1. There is a main directory for each platform, `ios` and `mac`
2. Each platform directory contains a list of numbered folders, where each folder stands for a specific CrashProbe test case.
3. Each test case folder contains a `data.json` folder with meta data for the test case and results for each service provider.
4. Each provider has one file per CPU architecture with the filename scheme `_{provider_name}_{cpu_arch}.crash`
	1. `{provider_name}` corresponds the item key value in the `data.json` file (here in lowercase)!
	2. `{cpu_arch}` can have the values `armv7` or `arm64` for iOS, and `x86_64` for OS X
5. Relevant provider data in `data.json`:
	1. `probes`: Array for all providers, with one item per provider. Please add a new item for a new provider.
	2. item key: Short name of the provider without spaces. This value is also used in the `.crash` filenames.
	3. `armv7`: Test result value for armv7 architecture (iOS only)
	4. `armv64`: Test result value for arm64 architecture (iOS only)
	5. `x86_64`: Test result value for X86-64 architecture (OS X only)
	6. Possible test result values:
		- `complete`: The crash report is 100% accurate and matches the control flow of the app. Exceptions also contain the exception reason.
		- `incomplete`: The crash report has missing frames, wrong class names, wrong methods, or shows wrong line numbers.
		- `wrong`: No crash report created or the crash report did not contain the method that caused the crash.
6. Each crash report should match the Apple style stack trace format as good as possible to make it easier to compare the data for viewers
7. Each item in the stack trace should be marked with pre-defined HTML to highlight any missing or wrong content. `{stack frame}` is the placeholder for each stack frame.
	1. Wrong line number: `<span class="cp-wrong">{stack frame} | Wrong line number</span>`
	2. Missing line number: `<span class="cp-wrong">{stack frame} | Missing line number</span>`
	3. Missing filename, and line number: `<span class="cp-wrong">{stack frame} | Missing filename, and line number</span>`
	4. Missing class: `<span class="cp-wrong">{stack frame} | Missing class</span>`
	5. Missing class, and method: `<span class="cp-wrong">{stack frame} | Missing class, and method</span>`
	6. Missing class, method, filename, and line number: `<span class="cp-wrong">{stack frame} | Missing class, method, filename, and line number</span>`
	7. Invalid / wrong / non-real frame: `<span class="cp-wrong">{stack frame} | Invalid frame</span>`
	8. Crashing frame missing: `<span class="cp-wrong">Missing frame that shows where the crash occured</span>`
	9. Exception details/reason missing: `<span class="cp-wrong">Missing exception details</span>`
	10. No report and app doesn't crash and locks up: `<span class="cp-wrong">No report, app locks up.</span>`
	11. No report: `<span class="cp-wrong">No report</span>`

# Contributing

We welcome crash reporting service providers to send us their test data via pull requests. Please use the following template for the pull request description:

```
Provider name:
Username and password for the account containing the data:
URL to the data on the providers website if available:

Date of testing:
SDK-Version:

Test environment per architecture: (Device name, OS Version)
```

# Test Process

1. Use one app version per CPU architecture, to more easily identify each test case for each CPU architecture
2. Test each architecture and test case one after another, so install, run the test, collect the data from the provider, continue with the next test. This makes sure that the result of each test case is identified correctly.


# Disclaimer

The test case data providers per service:

- Apple: [Bit Stadium GmbH](http://hockeyapp.net/)
- Bugsnag: [Bit Stadium GmbH](http://hockeyapp.net/)
- Crashlytics: [Bit Stadium GmbH](http://hockeyapp.net/)
- Crittercism: [Bit Stadium GmbH](http://hockeyapp.net/)
- HockeyApp: [Bit Stadium GmbH](http://hockeyapp.net/)
- New Relic: [Bit Stadium GmbH](http://hockeyapp.net/)
- Raygun: [Bit Stadium GmbH](http://hockeyapp.net/)
- Splunk MINT Express: [Bit Stadium GmbH](http://hockeyapp.net/)

# License

This test data is released under the MIT license.
