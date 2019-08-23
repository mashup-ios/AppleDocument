---
title:  WKWebView
framework: WebKit
sdks:  iOS 8.0+ / macOS 10.10+
created:   2019-08-22
updated: 2019-06-22
author: Juhee Kim
Copyright: © 2019 mashup-ios
---

# WKWebView
인앱 브라우저와 같은 interactive 웹 컨텐츠를 보여주는 객체

## Declaration
**iOS**
```swift
class WKWebView : UIView
```
**macOS**
```swift
class WKWebView : NSView
```
## Overview
> **중요**
>
> iOS 8.0과 OS X 10.10 부터 앱에 웹 컨텐츠를 추가하기 위해 WKWebView를 사용합니다. UIWebView나 WebView를 사용하지 마세요.

앱에 웹 콘텐츠를 넣기 위해 WKWebView class를 사용할 수 있습니다. 이를 위해 WKwebView 객체를 만들고, 이를 뷰로 설정하고 웹 컨텐츠를 불러오기 위해 request를 보내세요.

> **Note**
>
> WKWebView에 httpBody 컨텐츠를 넣을 수 있는 POST request를 만들 수 있습니다.

WKWebView 객체를 ```init(frame:configuration:)```로 만들고 난 뒤, 웹 컨텐츠를 불러올 필요가 있습니다. 로컬 HTML 파일을 불러오려면```loadHTMLString(_:baseURL:)``` 메소드를 사용하고 웹 컨텐츠를 불러오기 위해서는 ```load(_:)```를 사용하세요. 페이지 로딩을 중단하려면 ```stopLoading```을 사용하고, ```isLoading```속성을 사용하여 웹뷰가 로딩이 진행중인지의 여부를 확인할 수 있습니다. 웹 컨텐츠를 불러오는 걸 추적하기 위해서 WKUIDelegate 프로토콜을 채택하세요. 다음 코드목록은 WKWebView를 프로그램으로 만들어내는 예제입니다.

Listing 1
Creating a WKWebView programmatically
```swift
import UIKit
import WebKit
class ViewController: UIViewController, WKUIDelegate {

    var webView: WKWebView!

    override func loadView() {
        let webConfiguration = WKWebViewConfiguration()
        webView = WKWebView(frame: .zero, configuration: webConfiguration)
        webView.uiDelegate = self
        view = webView
    }
    override func viewDidLoad() {
        super.viewDidLoad()

        let myURL = URL(string:"https://www.apple.com")
        let myRequest = URLRequest(url: myURL!)
        webView.load(myRequest)
    }}
```
웹페이지의 히스토리를 통해 사용자가 앞으로 뒤로 가도록 허용하려면 ```goBack()```과 ```goForward()``` 메소드를 버튼 액션으로 사용하세요. ```canGoBack```과 ```canGoForward``` 속성으로 사용자가 그 방향(앞/뒤)으로 이동할 수 없을 때를 확인할 수 있습니다.

기본적으로, 웹 뷰는 자동적으로 웹 컨텐츠에 있는 전화번호를 전화 링크로 변환해줍니다. 전화 링크를 선택하면 전화 앱이 실행되고 번호를 보여줍니다. 이런 기본 동작을 끄기 위해서는, ```dataDetectoreTypes``` 프로퍼티를 전화번호 플래그를 포함하지 않는 ```WKdataDetectorTypes``` 로 설정하세요.

최초로 웹 컨텐츠에 표시될 크기를 ```setMagnification(_:centeredAt:)```를 사용하여 설정할 수 있습니다. 그 다음 사용자는 제스처를 사용하여 크기를 변경할 수 있습니다.

## Topics
### Webkit이 컨텐츠를 로딩할 수 있는지 없는지의 여부
```swift
class func handlesURLScheme(String) -> Bool
```
WebKit에서 기본적으로 특정 URL 스키마로 리소스를 불러올 수 있는지 여부를 반환합니다.

### WebView 초기화
웹 뷰를 초기화 하기 위한 설정
```swift
var configuration: WKWebViewConfiguration
```
```swift
// 정해진 frame과 설정을 가지는 webView 초기화
init(frame: CGRect, configuration: WKWebViewConfiguration)
init?(coder: NSCoder)
```
### Inspecting the View Information
```swift
// webView과 연결된 스크롤 뷰
var scrollView: UIScrollView
// 페이지 타이틀
var title: String?
// 활성화된 URL
var url: URL?
// 사용자의 user-agent String 값
var customUserAgent: String?
The custom user agent string.
// 최근 발생된 이동(navigation)에서의 SecTrustRef 객체
var serverTrust: SecTrust?
// 최근 발생된 이동(navigation)에서의 인증서 체인을 구성하는 객체들의 배열
var certificateChain: [Any]
An array of objects forming the certificate chain for the currently com
```
### Setting Delegates
```swift
// web view's navigation delegate.
var navigationDelegate: WKNavigationDelegate?
//web view's user interface delegate.
var uiDelegate: WKUIDelegate?
```
### Loading Content
```swift
// 네비게이션의 로딩된 정도를 추정한 값입니다.
var estimatedProgress: Double
// 페이지의 모든 리소스가 안전하게 암호화 된 연결을 통해 로드되었는지 여부를 나타내는 Bool 값입니다.
var hasOnlySecureContent: Bool
// 웹 페이지 내용과 기본 URL을 설정합니다.
func loadHTMLString(String, baseURL: URL?) -> WKNavigation?
// 뷰가 현재 내용을 로드 중인지 여부를 나타내는 Bool 값입니다.
var isLoading: Bool
// 현재 페이지를 다시 로드합니다.
func reload() -> WKNavigation?
func reload(Any?)
// 가능한 경우 캐시 유효성 검사 조건을 사용하여 end point 간 재 검증을 수행하여 현재 페이지를 다시 로드합니다.
func reloadFromOrigin() -> WKNavigation?
func reloadFromOrigin(Any?)
// 현재 페이지의 모든 리소스 로딩을 중단합니다.
func stopLoading()
func stopLoading(Any?)
// 웹 페이지 내용과 기본 URL을 설정합니다.
func load(Data, mimeType: String, characterEncodingName: String, baseURL: URL) -> WKNavigation?
// 파일 시스템의 요청 된 파일 URL로 이동합니다.
func loadFileURL(URL, allowingReadAccessTo: URL) -> WKNavigation?
```
### 컨텐츠 크기 조정
```swift
// 확대 제스쳐가 웹 뷰의 배율을 변경할 수 있도록 허용할 것인지 나타내는 Bool 값
var allowsMagnification: Bool
// 현재 페이지의 컨텐츠의 크기
var magnification: CGFloat
// 지정된 값으로 페이지 컨텐츠의 크기를 조정하고 지정된 지점에 결과를 중앙 정렬합니다.
func setMagnification(CGFloat, centeredAt: CGPoint)
```
### 이동
```swift
// 가로 스와이프 제스처가 역방향으로 이동하도록 허용할 것인지 여부를 나타내는 Bool값
var allowsBackForwardNavigationGestures: Bool
// 웹 뷰의 이동할 수 있는 페이지 목록
var backForwardList: WKBackForwardList
// back-forward에 뒤로 갈 수 있는 항목이 있는지 여부를 나타내는 Bool 값
var canGoBack: Bool
// back-forward 목록에 앞으로 갈 수 있는 항목이 있는지 여부를 나타내는 Bool 값
var canGoForward: Bool
// 링크를 누르면 미리보기가 표시되게 할 것인지 여부를 결정하는 Bool 값
var allowsLinkPreview: Bool
// back-forward에서 뒤 항목으로 이동합니다.
func goBack() -> WKNavigation?
func goBack(Any?)
// back-forward에서 앞 항목으로 이동합니다.
func goForward() -> WKNavigation?
func goForward(Any?)
// back-forward에서 항목을 이동하여 현재 항목으로 설정합니다.
func go(to: WKBackForwardListItem) -> WKNavigation?
// 요청된 URL로 이동합니다.
func load(URLRequest) -> WKNavigation?
```
### Executing JavaScript
```swift
// JavaScript String을 수행합니다.
func evaluateJavaScript(String, completionHandler: ((Any?, Error?) -> Void)?)
```
### Taking Snapshots
```swift
현재 뷰의 보여지는 부분의 snapshot을 생성합니다.
func takeSnapshot(with: WKSnapshotConfiguration?, completionHandler: (UIImage?, Error?) -> Void)
```

### 함께보기
* [original](https://developer.apple.com/documentation/webkit/wkwebview)
* [WKProcessPool](https://caution-dev.github.io/apple-docs/2019/06/25/WKProcessPool.html) : WKProcessPool 객체는 웹 컨텐츠 프로세스를 나타내는 풀입니다.
* [WKWindowFeatures](https://developer.apple.com/documentation/webkit/wkwindowfeatures.html) : 새 웹뷰가 요청될 때 포함 창에 대한 선택적 속성들을 지정합니다.
* [WKWebViewConfiguration](https://caution-dev.github.io/apple-docs/2019/06/25/WKWebViewConfiguration.html) : 웹 뷰를 초기화할 때 사용되는 속성 컬렉션입니다.
* [WKPreferences](https://caution-dev.github.io/apple-docs/2019/06/25/WKPreferences.html) : 웹 뷰에 대한 환경 설정을 캡슐화합니다.
* [WKUIDelegate](https://caution-dev.github.io/apple-docs/2019/06/25/WKUIDelegate.html) : 웹페이지를 대신해서 고유 사용자 인터페이스 요소들을 보여주는 메소드들을 제공합니다.

* [WKScriptMessage](https://caution-dev.github.io/apple-docs/2019/05/04/WKScriptMessage.html) : WKScriptMessage 객체에는 웹 페이지에서 보낸 메시지에 대한 정보가 들어 있습니다.
* [WKScriptMessageHandler](https://caution-dev.github.io/apple-docs/2019/05/06/WKScriptMessageHandler.html) : WKScriptMessageHandler 프로토콜을 준수하는 클래스는 웹 페이지에서 실행중인 JavaScript에서 메시지를 수신하는 메서드를 제공합니다.
