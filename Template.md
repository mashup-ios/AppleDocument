---
title:  WKUIDelegate
framework: WebKit
sdks:  iOS 8.0+ / macOS 10.10+
created:   2019-06-25
updated: 2019-06-25
author: Juhee Kim
Copyright: © 2019 mashup-ios
---


# WKUIDelegate

### Declaration
```swift
protocol WKUIDelegate
```

### Overview
이 프로토콜을 채택한 웹 뷰 사용자 인터페이스 위임자는 새창 열기를 제어하거나, 사용자가 요소를 클릭할 때 표시되는 기본 메뉴 항목의 동작을 늘리거나, 사용자 인터페이스 관련 작업들을 수행합니다. 이러한 메소드들은 JavaScript 또는 다른 플러그인 컨텐츠를 처리 한 결과를 호출 할 수 있습니다. 기본적인 웹 뷰 구현은 웹 뷰 마다 하나의 창을 사용하므로 일반적이지 않은 사용자 인터페이스는 아마 이 사용자 인터페이스 위임을 채택했다고 볼 수 있습니다.

### Topics
#### 새 웹 뷰 생성하기
```swift
func webView(WKWebView, createWebViewWith: WKWebViewConfiguration, for: WKNavigationAction, windowFeatures: WKWindowFeatures) -> WKWebView?
```

#### UI 패널 보여주기
```swift
// Javascript alert 패널을 보여줍니다.
func webView(WKWebView, runJavaScriptAlertPanelWithMessage: String, initiatedByFrame: WKFrameInfo, completionHandler: () -> Void)

// Javascript confirm 패널을 보여줍니다.
func webView(WKWebView, runJavaScriptConfirmPanelWithMessage: String, initiatedByFrame: WKFrameInfo, completionHandler: (Bool) -> Void)

// Javascript 텍스트 입력 패널을 보여줍니다.
func webView(WKWebView, runJavaScriptTextInputPanelWithPrompt: String, defaultText: String?, initiatedByFrame: WKFrameInfo, completionHandler: (String?) -> Void)
```

### 헤더
#### 서브헤더
```swift
코드
```

...

### 함께보기
* [original](https://developer.apple.com/documentation/webkit/wkuidelegate)
* [Pick&Pop](https://developer.apple.com/documentation/)
* []()
