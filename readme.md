# Update Aug 25, 2020
_[CloudFlare now has a beta of their macOS (and Windows) specific 1.1.1.1 app!](https://1.1.1.1/beta/) I encourage you to go try that instead of tinkering with this. I'll leave the code up in case it's helpful for anyone, but will be archiving the repository immediately. Thanks for the interest!_

# 1.1.1.1 on macOS
[CloudFlare has a cool app for iOS devices](https://itunes.apple.com/us/app/1-1-1-1-faster-internet/id1423538627) that enables easy switching to faster, private, and encrypted DNS queries. While switching DNS isn't exactly difficult, I found the user experience to be interesting - very easy to configure, just tap a switch and you're in.

Another interesting thing about it is that, unless they're doing some super custom stuff (which is totally possible), I'm gonna hazard a guess that it has to be built on top of `NetworkExtension` APIs. With the exception of smaller things like [NEDNSProxyProvider](https://developer.apple.com/documentation/networkextension/nednsproxyprovider), most of that stuff should work fine on macOS now... but very few companies ever take the time to take an almost-compatible iOS codebase and shim it for Cocoa/AppKit. The UI used on `1.1.1.1` isn't too difficult to do in macOS, though.

Thus, over the holidays I dug in to see how annoying it'd be to do. This implements a very basic VPN tunnel that sets DNS to go over 1.1.1.1, in a basic UI as a status bar menu app. Some screenshots are below. I probably won't be pursuing this further in lieu of working on other projects, so anyone out there should feel free to take this and extend it as they wish.

Also, side notes:

- There are two types of on-demand VPNs, which this uses - Personal and Enterprise. By using `NETunnelProvider`, the VPN profile counts as an Enterprise one, which trumps Personal in cases where two might get loaded.
- I wager CloudFlare probably goes much deeper than this, as some [old documentation buried on Apple's site indicates that connections that use POSIX Sockets and `CFSocket` technically won't go through an on-demand VPN](https://developer.apple.com/library/archive/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/CommonPitfalls/CommonPitfalls.html#//apple_ref/doc/uid/TP40010220-CH4-SW2). If this is still true, I can't imagine they'd have overlooked it like this approach does.
- While macOS supports `NetworkExtension` APIs, I have to wonder if the old `SystemConfiguration` APIs weren't slightly more useful for what's intended here.
- You probably want a true VPN instead of this, but this isn't a bad approach either in the grand scheme of things. Probably one where doing your research is worthwhile. :)

## Screenshots
![Disconnected](https://github.com/ryanmcgrath/1.1.1.1-macOS/blob/master/screenshots/disconnected.png?raw=true)
![Connected](https://github.com/ryanmcgrath/1.1.1.1-macOS/blob/master/screenshots/connected.png?raw=true)

## What else is here?
This repo could also be used as scaffolding/reference for a nibless Swift Cocoa app, if you're into that sorta thing. I personally think Interface Builder makes anyone who deals with UI in code (web devs, etc) groan out loud, so maybe this goes towards showing it's not _that_ difficult or outlandish to do otherwise.

- It implements a taskbar app with a custom `NSPopover` view in code.
- It implements a working `UISwitch` replacement, using a slightly-modified [JSSwitch](https://github.com/juliensagot/JSSwitch).

## License, etc
This is very much a "do-wtf-you-want-with-it" license. Code is as-is. I'd like to give props to [this list of TLDs by popmedic](https://gist.github.com/popmedic/cf9472aa8c2adda875a484c5a1c5da06), because compiling it myself would've been annoying. His code inspired some of the VPN portion as well; I spent enough time on it but couldn't come up with a better approach than his, so wound up just tweaking it slightly for my needs.

Questions can be directed to [ryan@rymc.io](mailto:ryan@rymc.io) or [@ryanmcgrath on Twitter](https://twitter.com/ryanmcgrath).
