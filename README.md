# I Don't Care About HSTS for Localhost!

Helps ease the pain of newer Chrome versions forcing HTTP Strict Transport Security for localhost, then caching via dynamic domain security policies if it ever works once, forcing HTTPS on local dev servers until "localhost" is manually reset via chrome://net-internals/#hsts every single time this happens. This installable policy for macOS fixes that via adding localhost to HSTSPolicyBypassList and CertificateTransparencyEnforcementDisabledForURLs.

## TL;DR

If you're seeing this on localhost dev servers:
![img/sadface.png](img/sadface.png?raw=true "img/sadface.png")
... and you're sick of deleting `localhost` from [chrome://net-internals/#hsts](chrome://net-internals/#hsts):
![img/net-internals.png](img/net-internals.png?raw=true "img/net-internals.png")
...install the [com.google.Chrome.mobileconfig](com.google.Chrome.mobileconfig) profile from this repo, restart Chrome, and it should work.

## Compatability

This is designed to work with Chrome >=78 on macOS (tested with macOS Monterey 12.3.1). Similar methods of setting this are available for Windows and Linux; see the official docs under [References and Further Reading](#references-and-further-reading) for more details on how to set those on each OS. https://appuals.com/how-to-clear-or-disable-hsts-for-chrome-firefox-and-internet-explorer/ also has some suggestions on permanent fixes for Firefox and Internet Explorer on Windows.

## Installation

1. Clone this repo
2. In a terminal in the cloned directory, run `open com.google.Chrome.mobileconfig`. Alternatively, in a Finder window, double-click the file `com.google.Chrome.mobileconfig`
3. You should see a notification in the upper-right that reads `Review the profile in System Preferences if you want to install it`
4. Navigate to  -> System Preferences -> Profiles
5. You should see the policy called `I Don't Care About HSTS for Localhost!` in the `Downloaded` section of the Profiles preferences pane. Click `Install...` and follow the prompts to authenticate and install the profile.
![img/policy.png](img/policy.png?raw=true "img/policy.png")
6. Quit all Google Chrome windows. One easy way to do this is to select the `Chrome` menu on the top bar and select `Quit Google Chrome`.
7. Verify that the policies are installed: re-open Google Chrome and navigate to [chrome://policy/](chrome://policy/) 
8. In the upper-right corner of the chrome://policy page, in the `Filter Policies by name` textbox, enter `hsts` and you should see the following:
![img/hstspolicybypasslist.png](img/hstspolicybypasslist.png?raw=true "img/hstspolicybypasslist.png")
9. In that same `Filter Policies by name` textbox, enter `url` and you should see the following:
![img/certificatetransparencyenforcementdisabledforurls.png](img/certificatetransparencyenforcementdisabledforurls.png?raw=true "img/certificatetransparencyenforcementdisabledforurls.png")


## References and Further Reading
- StackOverflow about this issue: https://stackoverflow.com/questions/38968510/how-to-permanently-exclude-localhost-from-hsts-list-in-google-chrome
- Chromium bug with details on why `defaults write com.google.Chrome HSTSPolicyBypassList -string "localhost"` doesn't work: https://bugs.chromium.org/p/chromium/issues/detail?id=859185
- The *right* way to fix this by generating a cert for use with local dev servers https://scatteredcode.net/debugging-on-localhost-with-hsts
- Official docs on HSTSPolicyBypassList: https://chromeenterprise.google/policies/?policy=HSTSPolicyBypassList
- Official docs on CertificateTransparencyEnforcementDisabledForUrls: https://chromeenterprise.google/policies/?policy=CertificateTransparencyEnforcementDisabledForUrls
- Python tool to convert a plist to a Configuration Profile https://github.com/timsutton/mcxToProfile
- Source file that [com.google.Chrome.mobileconfig](com.google.Chrome.mobileconfig) was generated from using mcxToProfile: [src/com.google.Chrome.plist](src/com.google.Chrome.plist)
- Some background on the problem and other ways to fix this in Chrome, Firefox, and Internet Explorer: https://appuals.com/how-to-clear-or-disable-hsts-for-chrome-firefox-and-internet-explorer/