# react-native-bypass-deeplink-with-notification (iOS)
**note: this repository is just a quick fix, not an elegant solution at all.

### Problem:
When press on notification bar (contain depp link url) and the iOS app is in closed state(terminated). Then `Linking.getInitialURL()` cannot receive notification url (url always null) in react-native side. 

### Cause:
in RCTLinkingManager.m , `getInitialURL` is waiting for some data in `bridge.launchOptions` property [#issue-5047(322290739)](https://github.com/facebook/react-native/issues/5047#issuecomment-322290739)

### Quick fix:
in RCTLinkingManager.m at `RCT_EXPORT_METHOD(getInitialURL)`, set `NSURL *initialURL = url;` 

while url is from  AppDelegate.m ::

`- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
  return [RCTLinkingManager application:application openURL:url options:options];
}`

### Manual:
1. clone this repository and move `lib` to your react-native project root
2. Xcode -> your project settings -> Build Phases -> Add( + button ) -> New Run Script Phase
3. put shell command `cp ../lib/LinkingIOS/RCTLinkingManager.m ../node_modules/react-native/Libraries/LinkingIOS/RCTLinkingManager.m`
4. Projduct -> Build (`/node_modules/react-native/Libraries/LinkingIOS/RCTLinkingManager.m` should be replaced with `/lib/LinkingIOS/RCTLinkingManager.m`)
5. Run project.
