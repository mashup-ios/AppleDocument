---
title:  DarkMode
framework: HIG
sdks:  iOS 13.0+ / macOS 10.14+
created: 2019-12-02
updated: 2019-12-02
author: Juhee Kim
Copyright: © 2019 mashup-ios
---


# Dark Mode

iOS 13.0 이후 버전부터, 사람들은 다크모드라고 부르는 어두운 시스템 화면 모드를 선택할 수 있습니다. 다크모드에서는 시스템이 모든 스크린, 뷰, 메뉴, 컨트롤들에 대한 보다 어두운 컬러 팔레트를 사용하게 되며, 어두운 배경에 비해 전경 콘텐츠를 돋보이게 하기 위해 더 선명한 컬러들을 사용합니다. 다크 모드는 모든 접근성 기능을 지원합니다.

사람들은 기본 인터페이스 스타일로 다크 모드를 선택할 수 있으며, 주변 조명이 낮을 때 설정을 사용하여 장치가 자동으로 다크 모드로 전환되도록 할 수 있습니다.

**컨텐츠에 집중하기** 다크 모드는 인터페이스의 컨텐츠 영역에 초점을 맞추므로 주변 UI가 백그라운드에 있는 동안 해당 컨텐츠가 눈에 띌 수 있습니다.


**밝은 색과 어두운 색으로 디자인을 테스트하기** 인터페이스가 두 가지 모드 모두 어떻게 표시되는지 확인하고 필요에 따라 디자인을 조정하세요. 한 모드에서 잘 작동하는 디자인도 다른 디자인에서는 작동하지 않을 수 있습니다.


**대비와 투명성 접근성 설정을 조정했을 때에도 다크 모드에서 콘텐츠를 편안하게 읽을 수 있는지 확인하기** 다크 모드에서 대비를 높이고 투명성을 줄이는 설정을 모두 껏다 켰다하면서 테스트를 해보아야 합니다. 어두운 배경 위의 어두운 텍스트 중 가독성이 떨어지는 곳을 찾을지도 모릅니다. 또한 다크 모드에서 대비를 높였을 때 어두운 텍스트와 어두운 배경 사이에 줄어든 시각적 대비를 발견할지도 모릅니다. 비록 시력이 좋은 사람들은 낮은 대조에도 텍스트를 읽을 수 있을지라도 시각 장애가 있는 사람들에겐 읽기 어려울 수 있습니다. 자세한 내용은 [색 및 대비](https://developer.apple.com/design/human-interface-guidelines/accessibility/overview/color-and-contrast/)를 참조하세요.

# 색상

어두운 모드의 색상 팔레트에는 모드 간 일관된 느낌을 유지할 수 있는 대비를 가진 어두운 배경색과 밝은 전경색이 포함되어 있습니다.

**현재 모드에 맞는 색을 사용하기** [구분자(seperator)](https://developer.apple.com/documentation/uikit/uicolor/3173139-separator)와 같은 의미적 색상(Sementic color)들은 현재 모드에 따라 자동으로 적용됩니다. 사용자 지정 색상이 필요한 경우 앱에 Color Set 에셋을 추가하고 현재 화면 모드에 맞게 색상의 밝은 색상과 어두운 색상을 지정합니다. 하드 코딩된 색상 값이나 모드에 따라 변경되지 않는 색상은 사용하지 않도록 합시다.

**모든 외관상 충분한 색상 대비를 보장하기** 시스템 정의 색상을 사용하면 전경과 배경 컨텐츠 간의 적절한 대비 비율을 보장할 수 있습니다. 사용자 지정 색상의 경우 특히 작은 텍스트의 경우 7:1의 대비 비율을 목표로 합니다. 자세한 내용은 [동적 시스템 색상](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#dynamic-system-colors)을 참조합니다.

**화이트 배경의 색상을 부드럽게 하기  줍니다. 어두운 모드에서 컨텐츠에 흰색 배경을 사용해야 하는 경우, 주변 어두운 컨텐츠에 대해 백그라운드에서 빛을 내지 않도록 약간 어두운 흰색 배경을 선택합니다. 

관련 지침은 [색상](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/)을 참조합니다.

### 이미지, 아이콘, 심볼 컬러

시스템은 다크 모드에서는 자동으로 멋져 보이는 [SF 심볼](https://developer.apple.com/design/human-interface-guidelines/sf-symbols/)와 밝고 어두운 외관에 최적화된 풀 컬러 이미지를 사용합니다.

**가능한 경우 SF 심볼를 사용합니다.** 심볼은 [동적 색상](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#dynamic-system-colors)을 사용하여 색상을 추가하거나 진동을 추가할 때 두 가지 화면 모드 모두에서 매우 적합합니다.

**필요한 경우 가볍고 어두운 외관을 위해 개별 글리프를 디자인하기** 밝은 모드에서 빈 윤곽선을 사용하는 글리프는 어두운 모드에서 채워진 솔리드 모양처럼 더 잘 보일 수 있습니다.

**풀 컬러 이미지와 아이콘이 잘 보이는지 확인하기** 밝은 모드와 어두운 모드 모두에서 좋아 보이는 동일한 asset을 사용하세요. 한 모드에서만 양호해 보이는 경우 asset을 수정하거나 별도의 밝은 모드 및 어두운 모드의 asset을 만드세요. asset catalog를 사용하여 명명된 단일 이미지로 asset을 결합하세요.

### 텍스트 컬러

Vibrancy는 어두운 배경에서 텍스트의 좋은 대조를 유지하는 데 도움이 될 수 있습니다.

**시스템에서 제공한 레이블 색상을 레이블에 사용하기** `primary`, `secondary`, `tertiary` 및 `quaternary` 레이블 색상은 밝은 모드과 어두운 모드에 자동으로 조정됩니다. 관련 지침은 [Typeography(유형도)](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/typography/)를 확인하세요.

**시스템 뷰를 사용하여 텍스트 필드 및 텍스트 보기를 그리기** 시스템 뷰 및 컨트롤을 사용하면 앱의 텍스트가 모든 배경에서 잘 보이게 되며, 진동 유무에 맞게 자동으로 조정됩니다. 시스템 제공 뷰를 사용하여 해당 텍스트를 표시할 수 있는 경우 텍스트를 직접 그리지 마세요. 개발자 지침을 보려면 [UITextField](https://developer.apple.com/documentation/uikit/uitextfield) 및 [UITextView](https://developer.apple.com/documentation/uikit/uitextview)를 참조합니다.

### 함께보기
* [original](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/dark-mode/)
* [Dark Mode 적용하기]()
