# AppleDocument
MashUp Apple Document Repository

한국어로 만나보는 Apple 공식문서!

> 의역, 오역 난무합니다. 피드백, PR 환영합니다!

## Convention
### Git
* 작업 전 이슈를 등록합니다. `Create/Update WKWebView`
* 작업전에 중복을 방지하기 위해 Project에서 해당 이슈를 In-progress 로 설정한 뒤 작업합니다.
* 개인 Repository로 Fork 해서 PR > Review > Merge 방식으로 진행합니다.
* PR을 작성할 때 해당 Issue 를 링크해서 연결될 수 있도록 합니다.

### URI Convention
* 손쉽게 문서를 검색할 수 있도록 기존 Apple Docs와 동일한 URI 방식을 따릅니다.
* 파일 명은 모두 소문자로 작성합니다.

#### Apple Docs
```
https://developer.apple.com/documentation/webkit/wknavigationdelegate
```

#### Mash Up AppleDocument
```
https://github.com/mashup-ios/AppleDocument/blob/master/webkit/wknavigationdelegate.md
```

### Content Convention
* 문서의 가독성을 높이기 위해 높임말과 외래어 표기는 하나의 문서 내에서 동일한 흐름으로 통일될 수 있도록 합니다.
#### Good
```
WKNavigationDelegate 프로토콜의 메서드는 웹 뷰에서 탐색 요청을 수락하고, 불러오고 및 완료하는 과정에서 트리거되는 사용자 지정 동작을 구현하는 데 도움이 됩니다.
웹 뷰가 웹 컨텐츠를 수신하기 시작했을 때 호출됩니다.
```

#### Bad
 > 웹 뷰/WebView, 됩니다/된다 의 혼용
```
WKNavigationDelegate 프로토콜의 메서드는 웹 뷰에서 탐색 요청을 수락하고, 불러오고 및 완료하는 과정에서 트리거되는 사용자 지정 동작을 구현하는 데 도움이 됩니다.
WebView가 웹 컨텐츠를 수신하기 시작했을 때 호출된다.
```

### Template
* Apple Docs와 유사한 형태를 가지면서, Repository에서 통일성을 가질 수 있도록 [Template](Template.md) 파일을 기본 베이스로 사용합니다.
