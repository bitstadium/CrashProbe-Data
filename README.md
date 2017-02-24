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

We welcome crash reporting service providers to send us their test data via pull requests. To create valid test data please use our CrashProbe client repo for either iOS or OSX test client which can be found here: [CrashProbe Client](https://github.com/bitstadium/CrashProbe)

Please also provide a link to a public repository on GitHub containing a configured version of CrashProbe including the SDK used for the test. This will make any result more transparent for every user and also speeds up the validation process on our side. There is no need to include the API-Keys of the SDK into the repository.

Example public repository: [https://github.com/bitstadium/CrashProbe-HockeyApp](https://github.com/bitstadium/CrashProbe-HockeyApp)

Example pull request: [https://github.com/bitstadium/CrashProbe-Data/pull/1](https://github.com/bitstadium/CrashProbe-Data/pull/1)

Please use the following template for the pull request description:

```
Provider name:
Username and password for the account containing the data:
URL to the data on the providers website if available:
Repository URL:

Date of testing:
SDK-Version:
Swift-Version:

Test environment per architecture: (Device name, OS Version)
```

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Contributor License

You must sign a [Contributor License Agreement](https://cla.microsoft.com/) before submitting your pull request. To complete the Contributor License Agreement (CLA), you will need to submit a request via the [form](https://cla.microsoft.com/) and then electronically sign the CLA when you receive the email containing the link to the document. You need to sign the CLA only once to cover submission to any Microsoft OSS project. 

# Test Process

1. Use one app version per CPU architecture, to more easily identify each test case for each CPU architecture
2. Test each architecture and test case one after another, so install, run the test, collect the data from the provider, continue with the next test. This makes sure that the result of each test case is identified correctly.


# Disclaimer

The test case data providers per service:

- Apple: [Bit Stadium GmbH](http://hockeyapp.net/)
- Bugsee: [Bugsee, Inc](https://www.bugsee.com)
- Bugsnag: [Bugsnag, Inc](https://bugsnag.com)
- Crashlytics: [Bit Stadium GmbH](http://hockeyapp.net/)
- Crittercism: [Bit Stadium GmbH](http://hockeyapp.net/)
- HockeyApp: [Bit Stadium GmbH](http://hockeyapp.net/)
- New Relic: [Bit Stadium GmbH](http://hockeyapp.net/)
- Raygun: [Raygun Limited](https://raygun.com)
- Splunk MINT Express: [Bit Stadium GmbH](http://hockeyapp.net/)

# License

This test data is released under the MIT license.
