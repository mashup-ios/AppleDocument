---
title: Add Result to the Standard Library
created: 2019-09-29
updated: 2019-09-29
author: Hyeontae Kim
Copyright: © 2019 mashup-ios
---

# Add Result to the Standard Library

- Proposal: [SE-0235](https://github.com/apple/swift-evolution/blob/master/proposals/0235-add-result.md)
- Author: [Jon Shier](https://github.com/jshier)
- Review Manager: [Chris Lattner](https://github.com/lattner)
- Status: **Implemented (Swift 5)**
- Implementation: [apple/swift#21073](https://github.com/apple/swift/pull/21073), [apple/swift#21225](https://github.com/apple/swift/pull/21225), [apple/swift#21378](https://github.com/apple/swift/pull/21378)
- Review: ([initial review](https://forums.swift.org/t/se-0235-add-result-to-the-standard-library/17752)) ([second review](https://forums.swift.org/t/revised-se-0235-add-result-to-the-standard-library/18371)) ([acceptance](https://forums.swift.org/t/accepted-with-modifications-se-0235-add-result-to-the-standard-library/18603))

## Introduction ( 소개 )

throw, try and catch를 사용하는 swift의 현재 error-handling은 명백한 구문 그리고 런타임을 통해서 자동화 되어있고 동시에 error handling 을 제공하고 있다. 하지만 이는 유연성이 부족하여 모든 에러 증식과 핸들링을 처리하기에는 부족하다. Result 타입은 다른 언어에서 그리고 Swift 커뮤니티에서 수동으로 에러 증식을 대응하기 위해서 전형적으로 사용되고 있다. 따라서 이 제안을 통해서 Swift Standard Library 에 해당 타입을 추가하고자 한다.

## Motivation ( 동기 )

[Swift's Error Handling Rationale and Proposal document](https://github.com/apple/swift/blob/master/docs/ErrorHandlingRationale.rst)  는 Swift에서 error handling 의 사례에 대한 추론과 높은 수준의 세부사항을 제시하고 있다. Error를 따르는 타입은 do try catch 에 의해서 전파되고 다뤄질 수 있다. 이론적인 문서는 이런 모델을 형식화 되고 자동으로 전파되는 에러 시스템이라고 언급한다. 하지만, 이 시스템은 몇가지 단점을 가지고 있고, 그중 몇몇은 이론적인 문서에도 언급 되었다. 즉, 비동기 작업, 더욱 복잡한 error handling 혹은 Error 타입을 따르지 않는 실패 값에 대해서는 구성을 할 수가 없다.  수동으로 전파되는 오류 유형의 추가 유연성을 통해서 이러한 단점을 해결할 수 있다. 즉 Result<Value, Error>.

( propagating 을 전파되는 이라고 해석하였음, 에러가 타고 넘어가는 형식을 propagation이라고 말하는 것 같음 ) 

## Proposed solution ( 제안된 해결책 )

```swift
public enum Result<Success, Failure: Error> {
    case success(Success), failure(Failure)
}
```

Result<Success, Failure>은 현재와 미래 사이의 error handling에 대한 실용적인 절충안이다. .failure case로 캡쳐된 Failure 타입은 실패한 계산 결과를 수동적으로 전파하려는 Result의 본래 의도를 강조하고 단순화하기 위해서 Error 타입으로 제약되어 있다. 

### Usage ( 사용법 )

**Asynchronous APIs ( 비동기 API )** 

대부분, 애플과 Foundataion API에서 많이 볼수 있듯이, Result 는 비동기 completion handlers 에서 이상하게 전달되는 파라미터들을 통합할 수 있다. 예를 들어서, URLSession의 completion hanlder는 3개의 옵셔널 파라미터를 가진다.

```swift
func dataTask(with url: URL, completionHandler: @escaping (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask
```

그리고 이 방식은 결과를 우아하게 처리하기 힘들다 :

```swift
URLSession.shared.dataTask(with: url) { (data, response, error) in
    guard error != nil else { self.handleError(error!) }
    
    guard let data = data, let response = response else { return // Impossible? }
    
    handleResponse(response, data: data)
}
```

이 코드는 몇 줄 없지만, asynchronouse APIs 의 error hanldling에 대한 약점을 그대로 드러내고 있다. 강제 언래핑 뿐만 아니라, 아마도 불가능한 시나리오에 대해서도 만들어질 수 있다. 만약에 response 혹은 data 가 nil인 경우에는 어떻게 되는가? 그리고 이 경우가 가능하기는 한가? 그렇지 않을 것이다. 하지만 스위프트는 현재 이러한 불가능한 상황을 명시하는 것이 부족하다. Result 를 사용한다면 동일한 시나리오에 대해서 훨씬 더 우아한 처리를 가능하게 한다.

```swift
URLSession.shared.dataTask(with: url) { (result: Result<(response: URLResponse, data: Data), Error>) in // Type added for illustration purposes.
    switch result {
    case let .success(success):
        handleResponse(success.response, data: success.data)
    case let .error(error):
        handleError(error)
    }
}
```

해당 API는 명백하게 의도된 result ( error 혹은 data 와 response, 둘 다 있거나 없는 경우는 없다. )를 표현하고, 그들을 명백하게 처리할 수 있다.

**More General Usage ( 더욱 일반적인 사용 )**

더욱 일반적으로, Result 가 error handling와 주변의 API를 더욱 우아하게 할 수 있는 몇가지 경우가 있다.

**Delayed Handling ( 지연된 핸들링 )**

개발자가 즉시 throwing function을 실행하고 싶지만 나중에 오류 처리를 해야하는 경우가 있다. 현재, 만약 그들이 에러를 보존하고 싶고, completion handlers에서 보이는 것처럼 값과 에러를 분리하고 싶다면. 이를 위해 하나의 함수 바깥에서 결과를 저장하는 것은 개발자들을 불쾌하게 할 것이다.

```swift
// Properties 
var configurationString: String?
var configurationReadError: Error?

do {
    string = try String(contentsOfFile: configuration)
} catch {
    readError = error
}

// Sometime later...

func doSomethingWithConfiguration() {
    guard let configurationString = configurationString else { handle(configurationError!) }
    
    ... 
}
```

이를 (string: String?, error: Error?) 를 사용하므로서 조금 더 명백하게 만들 수 있다, 하지만 비동기 작업의 경우에는 그대로 이슈를 가지고 있을 것이다. Result를 사용하는 것이 명백한 정답이다, 특히 throwing function에서 결과를 만드는 편리한 API가 그러하다.

```swift
let configuration = Result { try String(contentsOfFile: configuration) }

// Sometime later...

func doSomethingWithConfiguration() {
    switch configuration {
        ...
    }
}
```

**Separating Errors ( 에러의 분리 )**

이는 때때로 throwable functions를 여러 에러들을 명백하게 구분하고 싶은 경우 유용하게 사용할 수 있도록 한다. 특히 에러가 어떤 필요한 정보들을 담고 있지 않을 때, 혹은 개발자들이 이런 체크를 하고 싶지 않을 때. 예를 들어서, 우리가 파일을 읽는 경우 가능한 여러 에러를 명백하게 나누고 싶을 때:

```swift
do {
    handleOne(try String(contentsOfFile: oneFile))
} catch {
    handleOneError(error)
}

do {
    handleTwo(try String(contentsOfFile: twoFile))
} catch {
    handleTwoError(error)
}

do {
    handleThree(try String(contentsOfFile: threeFile))
} catch {
    handleThreeError(error)
} 
```

이런 경우는 Result에서 조금 더 명백하게 표현될 수 있다.

```swift
let one = Result { try String(contentsOfFile: oneFile) }
let two = Result { try String(contentsOfFile: twoFile) }
let three = Result { try String(contentsOfFile: threeFile) }

handleOne(one)
handleTwo(two)
handleThree(three)
```

Result의 추가적인 편의 API는 이러한 경우들을 훨씬 더 우아하게 만들 수 있다.

**추가 내용은 생략**

### 함께보기
* [original](https://github.com/apple/swift-evolution/blob/master/proposals/0235-add-result.md)
* [How to use Result in Swift 5](https://www.hackingwithswift.com/articles/161/how-to-use-result-in-swift)
* [Swift5의 Result Type 사용하기](https://eunjin3786.tistory.com/47)
* [Result](https://developer.apple.com/documentation/swift/result)
