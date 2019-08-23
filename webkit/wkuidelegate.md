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

#### Web View 닫기
```swift
// DOM 창이 성공적으로 닫혔을 때 호출되는 메소드입니다.
func webViewDidClose(WKWebView)
```

#### Upload 패널 보여주기
```swift
// 파일 업로드 패널을 보여주는 메소드입니다.
func webView(WKWebView, runOpenPanelWith: WKOpenPanelParameters, initiatedByFrame: WKFrameInfo, completionHandler: ([URL]?) -> Void)
```

#### 감압 터치 액션 수신하기
```swift
// 주어진 요소가 미리보기를 보여주어야 하는지 결정합니다.
func webView(WKWebView, shouldPreviewElement: WKPreviewElementInfo) -> Bool

// 사용자가 peek 액션을 수행했을 때 호출됩니다.
func webView(WKWebView, previewingViewControllerForElement: WKPreviewElementInfo, defaultActions: [WKPreviewActionItem]) -> UIViewController?

// 사용자가 미리보기에서 pop 액션을 수행했을 때 호출됩니다.
func webView(WKWebView, commitPreviewingViewController: UIViewController)
Called when the user performs a pop action on the preview.
```

### 함께보기
* [original](https://developer.apple.com/documentation/webkit/wkuidelegate)
* [Pick&Pop](https://developer.apple.com/documentation/uikit/peek_and_pop/implementing_peek_and_pop)
* [WKNavigationDelegate](https://caution-dev.github.io/apple-docs/2019/06/25/WKNavigationDelegate.html)
  * 웹뷰에서 탐색 요청을 수락하고, 불러오고 및 완료하는 과정에서 트리거되는 사용자 지정 동작을 구현하는 데 도움이 됩니다.
* [WKProcessPool](https://caution-dev.github.io/apple-docs/2019/06/25/WKProcessPool.html)
  * WKProcessPool 객체는 웹 컨텐츠 프로세스를 나타내는 풀입니다.
* [WKWindowFeatures](https://caution-dev.github.io/apple-docs/2019/06/25/wkwindowfeatures.html)
  * 새 웹뷰가 요청될 때 포함 창에 대한 선택적 속성들을 지정합니다.
* [WKPreferences](https://caution-dev.github.io/apple-docs/2019/06/25/WKPreferences.html)
  * 웹 뷰에 대한 환경 설정을 캡슐화합니다.
* [WKWebView](https://caution-dev.github.io/apple-docs/2019/06/25/WKWebView.html)
  * 인앱브라우저와 같이 상호작용할 수 있는 웹 컨텐츠 객체입니다.
* [WKWebViewConfiguration](https://caution-dev.github.io/apple-docs/2019/06/25/WKWebViewConfiguration.html)
  * 웹 뷰를 초기화할 때 사용되는 속성 컬렉션입니다.

