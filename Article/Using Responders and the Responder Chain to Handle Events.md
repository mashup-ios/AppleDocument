---
title:  Using Responders and the Responder Chain to Handle Events
framework: iOS
sdks:  -
created:   2019-11-26
updated: 2019-11-26
author: Hyeontae Kim (onemoon)
Copyright: © 2019 mashup-ios
---

# Using Responders and the Responder Chain to Handle Events

**이벤트를 다루기 위해서 Responder와 Responder Chain 을 사용하자**



Learn how to handle events that propagate through your app.

앱에 전달되는 이벤트를 어떻게 다루는지 알아보자



## OverView

**개요**



Apps receive and handle events using *responder objects*. A responder object is any instance of the [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder) class, and common subclasses include [`UIView`](https://developer.apple.com/documentation/uikit/uiview), [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller), and [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication). Responders receive the raw event data and must either handle the event or forward it to another responder object. When your app receives an event, UIKit automatically directs that event to the most appropriate responder object, known as the *first responder*.

앱은 Responder 객체를 통해서 이벤트를 받고 다룬다. responder 객체란 UIResponder class를 상속받은 객체를 말을 하는데 보통 UIView, UIViewController, UIApplication 등이 있다. 이벤트를 받은 Responder는 반드시 이벤트를 다루거나 다른 responder 객체로 이를 전달해야 한다. 앱에서 이벤트를 받았을 때, UIKit 에서는 자동으로 이벤트가 가장 적절한 respnder object로 향한다, 이는 first responder 로 알려져 있다.



Unhandled events are passed from responder to responder in the active *responder chain*, which is the dynamic configuration of your app’s responder objects. [Figure 1](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events#3004381) shows the responders in an app whose interface contains a label, a text field, a button, and two background views. The diagram also shows how events move from one responder to the next, following the responder chain.

다뤄지지 않은 이벤트는 활성화된 responder chaing에 있는 responder에서 responder 전달된다. 1번 그림은 label과 view등의 인터페이스를 가진 앱의 responders를 보여준다. 또한 해당 다이어그램은 이벤트가 responder chain을 통해서 어떻게 다른 responder로 전달되는지 보여준다.



**Figure 1** Responder chains in an app

![A flow diagram: On the left, a sample app contains a label (UILabel), a text field for the user to input text (UITextField), and a button (UIButton) to  press after entering text in the field. On the right, the flow diagram shows how, after the user pressed the button, the event moves through the responder chain—from UIView, to UIViewController, to UIWindow, UIApplication, and finally to UIApplicationDelegate.](https://docs-assets.developer.apple.com/published/7c21d852b9/f17df5bc-d80b-4e17-81cf-4277b1e0f6e4.png)



If the text field does not handle an event, UIKit sends the event to the text field’s parent `UIView` object, followed by the root view of the window. From the root view, the responder chain diverts to the owning view controller before directing the event to the window. If the window cannot handle the event, UIKit delivers the event to the `UIApplication` object, and possibly to the app delegate if that object is an instance of `UIResponder` and not already part of the responder chain.

만약 text field 가 이벤트를 처리하지 않는다면, UIKit은 이벤트를 text field's 의 부모인 uiview 객체로 전달한 다음 window의 root view 로 전달된다. root view에서, chain은 window로 가기 전 소유한 viewcontroller로 바뀐다. 만약 window가 event를 처리할 수 없다면, UIKit은 이벤트를 이미 responder chain에 있지 않고, UIResponder 의 인스턴스인 UIApplication 객체로 보낸다.



## Determining an Event's First Responder

**이벤트의 First Responder를 결정하는 것**



UIKit designates an object as the first responder to an event based on the type of that event. Event types include:

UIKit은 객체를 이벤트 타입에 따라서  first responder 로 지정한다.  모든 타입의 이벤트가 포함된다.

| Event type            | First responder                           |
| :-------------------- | :---------------------------------------- |
| Touch events          | The view in which the touch occurred.     |
| Press events          | The object that has focus.                |
| Shake-motion events   | The object that you (or UIKit) designate. |
| Remote-control events | The object that you (or UIKit) designate. |
| Editing menu messages | The object that you (or UIKit) designate. |

>Note
>
>Motion events related to the accelerometers, gyroscopes, and magnetometer do not follow the responder chain. Instead, Core Motion delivers those events directly to the designated object. For more information, see [Core Motion Framework](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/CoreServicesLayer/CoreServicesLayer.html#//apple_ref/doc/uid/TP40007898-CH10-SW27)
>
> 모션 이벤트는 accelerometers, gyroscoppes, and magnetometer와 관련이 있고, responder chain 을 따르지 않는다. 대신에 Core Motion 은 해당 이벤트들을 지정된 객체에 전달한다. 더 자세한 내용은  [Core Motion Framework](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/CoreServicesLayer/CoreServicesLayer.html#//apple_ref/doc/uid/TP40007898-CH10-SW27)



Controls communicate directly with their associated target object using action messages. When the user interacts with a control, the control sends an action message to its target object. Action messages are not events, but they may still take advantage of the responder chain. When the target object of a control is `nil`, UIKit starts from the target object and traverses the responder chain until it finds an object that implements the appropriate action method. For example, the UIKit editing menu uses this behavior to search for responder objects that implement methods with names like [`cut(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354193-cut), [`copy(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354191-copy), or [`paste(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354189-paste).

Controls 그들이 연결된 대상 객체들과 액션 메세지를 통해서 대화한다. 유저가 control 과 상호작용을 할 때 control은 액션 메세지를 대상 객체에 보낸다. 액션 메세지는 이벤트가 아니다, 하지만 responder chain에서의 이점은 그대로 가지고 있다. 대상 객체가 nil인경우, UIKit은 적절한 액션이 구현된 객체를 찾을 때 까지 대상 객체에서 responder chain을 따라서 찾는다. 예를 들어서, UIkit이 메뉴를 수정할 때 cut, copy, paste 와 같은 메서드가 구현된 객체를 찾을 때 까지 responder chain을 따라간다. 



Gesture recognizers receive touch and press events before their view does. If a view's gesture recognizers fail to recognize a sequence of touches, UIKit sends the touches to the view. If the view does not handle the touches, UIKit passes them up the responder chain. For more information about using gesture recognizer’s to handle events, see [Handling UIKit Gestures](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures).

Gesture recognizer들은 터치 혹은 누르는 이벤트를 그들의 view가 인식하기 전에 인식한다. 만약 view의 gesture recognizer가 일련의 터치를 인식하는 것에 실패했다면, UIKit 은 그들을 responder chain 에 전달한다. gesture recognizer와 이벤트 핸들링에 대한 보다 자세한 내용은, see [Handling UIKit Gestures](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures)



## Determining Which Responder Contained a Touch Event

**터치 이벤트를 포함하는 응답자 결정**



UIKit uses view-based hit-testing to determine where touch events occur. Specifically, UIKit compares the touch location to the bounds of view objects in the view hierarchy. The [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method of [`UIView`](https://developer.apple.com/documentation/uikit/uiview) traverses the view hierarchy, looking for the deepest subview that contains the specified touch, which becomes the first responder for the touch event.

UIKit은 어디에 터치가 이벤트가 발생했는지 결정하기 위해서 view-based hit-testing을 사용한다. 특히, UIKit은 view hierarchy 안에서 view 객체의 bound와 터치한 위치를 비교한다. hitTest라는 UIView의 메소드는 view hierarchy를 따라가는데, 특정한 터치를 포함하는 가장 깊은 subview를 찾는다, 이것이 터치 이벤트의 first responder 이다.



> Note
>
> If a touch location is outside of a view’s bounds, the [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method ignores that view and all of its subviews. As a result, when a view’s [`clipsToBounds`](https://developer.apple.com/documentation/uikit/uiview/1622415-clipstobounds) property is `false`, subviews outside of that view’s bounds are not returned even if they happen to contain the touch. For more information about the hit-testing behavior, see the discussion of the [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method in [`UIView`](https://developer.apple.com/documentation/uikit/uiview).
>
> 만약 터치 이벤트가 view's bound의 바깥이라면, hitTest 메서드는 해당 뷰와 하위 뷰들을 무시한다. 그 결과로, view's clipToBounds가 false인 경우, 해당 view's bounds 바깥에 있는 하위뷰들은 그들이 터치를 포함하고 있더라도 반환되지 않는다. hit-testing 행위에 대해서 더 알고싶다면, see the discussion of the [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method in [`UIView`](https://developer.apple.com/documentation/uikit/uiview).



When a touch occurs, UIKit creates a [`UITouch`](https://developer.apple.com/documentation/uikit/uitouch) object and associates it with a view. As the touch location or other parameters change, UIKit updates the same `UITouch` object with the new information. The only property that does not change is the view. (Even when the touch location moves outside the original view, the value in the touch’s [`view`](https://developer.apple.com/documentation/uikit/uitouch/1618109-view) property does not change.) When the touch ends, UIKit releases the `UITouch` object.

터치가 발생하고, UIKit이 UITouch 객체를 만들고 view와 연관시킨다. 터치의 위치나 다른 인자가 변경되는 경우, UIKit은 동일한 UITouch 객체를 새로운 정보로 업데이트 한다. 유일하게 변경되지 않는 속성이 view이다. (심지어 터치가 기존 뷰의 바깥으로 이동되었을 때에도, touch의 view 프로퍼티는 변하지 않는다.) 터치가 끝났을 때, UIKit은 UITouch 객체를 해제시킨다.



## Altering the Responder Chain

**Responder Chain 의 변경**

You can alter the responder chain by overriding the [`next`](https://developer.apple.com/documentation/uikit/uiresponder/1621099-next) property of your responder objects. When you do this, the next responder is the object that you return.

단순히 responder chain의 next 속성만 덮어 씌운다면 responder chain을 변경할 수 있다. 이 행위를 했을 때, next responder는 너가 반환한 객체이다.



Many UIKit classes already override this property and return specific objects, including:

- [`UIView`](https://developer.apple.com/documentation/uikit/uiview) objects. If the view is the root view of a view controller, the next responder is the view controller; otherwise, the next responder is the view’s superview.
- [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller) objects.
  - If the view controller’s view is the root view of a window, the next responder is the window object.
  - If the view controller was presented by another view controller, the next responder is the presenting view controller.
- [`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow) objects. The window's next responder is the [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) object.
- [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) object. The next responder is the app delegate, but only if the app delegate is an instance of [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder) and is not a view, view controller, or the app object itself.

많은 UIKit 클래스들은 이미 해당 속성을 덮어썼고, 특정한 객체를 반환한다, 아래를 포함한다.

- UIKit 객체들. 만약 view가 view controller의 root view일 때, next responder는 view controller가 된다. 그렇지 않으면, next responder는 view의 상위 뷰가 된다.
- UIViewController 객체들.
  - 만약 view controller의 view가 window의 root view라면, next responder는 window 객체가 된다.
  - 만약 view controller가 다른 뷰에 의해서 띄워졌다면, next responder는 띄워진 view controller가 된다.
- UIWindow 객체들. window의 next responder는 UIApplication 객체가 된다.
- UIApplication 객체. next responder는 app delegate이다, 하지만 오직 app delegate가 UIResponder의 인스턴스인 경우이며 view, view controller 그리고 app 객체 그 자체가 아닌 경우일 뿐이다.




### Reference

[Apple's Article - Using Responders and the Responder Chain to Handle Events](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)


#### author

[onemoon](https://github.com/onemoonstudio)

