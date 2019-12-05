---
title:  Xcode 11 Release Notes
framework: Xcode
sdks:  iOS 13.0+ / macOS 10.15+
created:   2019-10-03
updated:   2019-10-03
author: Juhee Kim
Copyright: © 2019 mashup-ios
---


#### Article
## Xcode 11 Release Notes
새 기능을 사용하려면 앱을 업데이트하고 API 변경 사항에 대해 앱을 테스트하세요.

### OverView
iOS 13, macOS Catalina 10.15, watchOS 6, 그리고 tvOS 13 SDK를 포함한 Xcode 11이 Mac App Store에 출시되었습니다. Xcode 11은 iOS 13.1이 구동되는 기기를 개발할 수 있도록 지원합니다. Xcode 11은 iOS 8 이후, tvOS 9 이후, watchOS 2 이후의 디버깅을 지원합니다. Xcode 11은 macOS Mojave 10.14.4 이상이 구동되는 맥을 요구합니다.

> **Note**
> iOS 11에서 실행할 때 런타임에 지정된 색상을 찾을 수 없는 Asset Catalog 버그가 수정되었습니다.

### **General**
### 새로운 기능들
* Xcode 11은 [SwiftUI](https://developer.apple.com/documentation/swiftui) 개발을 지원합니다. (22843503)

> **Note**
> SwiftUI 미리보기와 Inspector는 macOS Catalina 10.15 이상에서 구동될 때에만 사용할 수 있습니다.

* Xcode 11은 Mac Catalyst에 대한 지원을 추가하여 iPad 앱을 Mac에 빌드할 수 있습니다. (43577997)

> **Note**
> iPad 앱은 MacOS Catalina 10.15에서 실행될 때만 Mac용으로 빌드되도록 구성할 수 있으며 이전 버전의 MacOS에서는 My Mac을 구동 대상으로 사용할 수 없습니다.

* 이제 시스템 외관 설정과 관계 없이 Xcode의 외관을 변경할 수 있다. (41165587)
* Xcode는 Organizer 창이나 명령줄에서 `xcodebuild` 또는 `xcrun altool`로 앱을 업로드할 수 있도록 지원합니다. 애플리케이션 로더는 더 이상 Xcode에 포함되지 않습니다. (29008875)
* MacOS의 LaunchServices는 이제 Xcode에 내장된 Instruments, Simulator 및 기타 개발자 도구를 시작할 때 선택된 Xcode를 존중합니다. 예를 들어 Finder에서 Instruments 두 번 클릭하면, 선택한 Xcode에 대한 Instruments 버전이 실행됩니다. 명령줄에서 `xcode-select`와 함께 사용할 Xcode를 변경할 수 있습니다. (6757601)
* `Editor`를 `Assistant Editor`가 없어도 임의의 창에 추가할 수 있다. `Editor`는 상단 메뉴 바의 "Add Editor" 버튼이나 File > New > Editor 명령을 사용하여 추가할 수 있다. 각 `Editor`는 `Editor` 또는 `Editor Option` 메뉴의 명령을 통해 독립적으로 자신의 `Assistant Editor` 또는 `Canvas`를 표시할 수 있습니다. 이 두 가지 모드는 사용 가능한 경우 관련 내용을 자동으로 표시합니다. 여러 `Editor`를 사용할 때 View > Editor > Focus 명령을 사용하여 임시로 활성 `Editor`를 확장하여 전체 창을 채울 수 있으며, 다른 `Editor`를 숨길 수 있다. 또한 "Show Editor Only"는 지정된 `Editor`를 재설정하여 해당 `Editor`에서 활성화된 추가 보기를 모두 해제합니다. 소스 제어 지원하기 위해 Toolbar의 Code Review 버튼이 Comparison Editor로 대체되었습니다. 이제 소스 편집기의 `Editor` 메뉴에서 "Show Authors" 명령을 사용할 수 있다. SCM 로그는 현재 검사 구역에 있다. (43806898)

### 알려진 이슈
* URLForItemWithPersistentIdentifier을 구현하는 Objective-C iOS File Provider 응용 프로그램 확장자를 구축하는 경우 Xcode는 이 콜백 메서드가 더 이상 사용되지 않으며 호출되지 않을 것이라는 경고를 내보냅니다. 이 경고는 부정확하며, 이 메서드는 `deprecated` 되지 않았으며, 적절한 시간에 호출됩니다. (54487300)

### 해결된 이슈
* 시스템 테마는 dark이고 Xcode는 light 일 때 이슈 텍스트가 light color로 보이는 문제 해결(48230278)



### 함께보기
* [original](https://developer.apple.com/documentation/xcode_release_notes/xcode_11_release_notes)
