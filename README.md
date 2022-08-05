# KeenCodeStyleGuide

## 참고
- [Apple API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/#naming)
- [토미의 개발노트 - 팀에서 사용 중인 Swift Style Guide](https://jusung.github.io/Swift-Code-Convention/)
- [스타일쉐어 컨벤션](https://github.com/StyleShare/swift-style-guide)
- [에어비엔비 컨벤션](https://github.com/airbnb/swift)

----

## 1. 코드 포메팅
### 1) import
- 알파벳 순으로 정렬합니다.
- 해당 파일에서 필요로 하는 프레임워크/라이브러리는 모두 작성합니다.



### 1-2) 작성 순서
- class 안에 코드를 작성 시에는 다음 순서대로 작성합니다.
- 열거형
- UI 관련 (IBOutlet 이나 코드베이스의 컴포넌트)
- 상수
- 변수
  - 클로자로 할당하는 변수 선언
- View Life-Cycle (viewDidLoad, viewWillAppear ...)
- 기타 함수
- 액션 함수
- Extension

```swift
class ViewController: UIViewController {

	enum someType {
		case hide
		case show
	}

	let titleLabel: UILabel = UILabel()

	lazy var submitButton: UIButton = {
		let button = UIButton()
		return button
	}()
	
	override viewDidLoad() {
		super.viewDidLoad()

		// some code..
	}

	func someFunction() {
		// some code..
	}

	@objc func someAction() {
		// some code..
	}

}

extension ViewController: SomeDelegate {
	// some code..
}
```


### 1-3) 빈줄
- 빈줄에는 공백이 포함되지 않도록 합니다.
- 함수의 끝 부분 등 불필요한 빈줄은 남기지 않습니다.
- 함수와 함수 사이에는 빈줄 1줄을 둡니다.
  - 함수1 -> 함수2에 대한 주석 -> 함수2 와 같이 작성하고자 한다면
  - 함수1 -> 빈줄 -> 함수2에 대한 주석 + 함수2 로 작성합니다.
- for, if, switch, class, struct, extension이 시작할 때 구문 앞에 빈줄 한줄을 둡니다.
- 화면 전환 함수(present, dismiss, push, pop) 가 포ㅓ함된 구문이 내부가 3줄 이상일 때, 앞에 빈줄 한줄을 둡니다.

```swift
class SomeViewController: UIViewController {
	var user: User?

	...

	func firstFunction() {
		print("i'm first!")

		for _ in 5 {
			print("hi")
		}
	}

	// its second function
	func secondFuction() {
		print("i'm second.")
	}

	@objc func onCancel() {
		self.dismiss(animated: true)
	}

	@objc func onSave() {
		guard let name = user.name else { return }
		UserDefaults.standard.set(name, forKey: "NAME")

		self.dismiss(animated: true)
	}

	...
}
```

- super.viewDidLoad(), super.viewWillAppear() 뒤에는 빈줄 한줄을 둡니다.


### 1-4) 들여쓰기
- 탭 간격은 4space 로 합니다.
- 들여쓰기는 Xcode에서 제공하는 ^ + i 를 눌렀을 때, 적용되는 space를 사용합니다.



### 1-5) 띄어쓰기
- 콜론(:)을 사용할 때는 콜론의 오른쪽에만 공백을 둡니다.
- 삼항 연산자의 경우엔 앞뒤로 띄웁니다.
- 콤마(,) 뒤에는 공백을 둡니다.
```swift
let number: Int = 3
let isOdd: Bool = number % 2 == 1 ? true : false
let lotto: Int = [1, 10, 32, 40, 22, 12]
```


### 1-6) 반복적으로 사용되는 컴포넌트 값 모듈화하기
- 여러 파일에서 공통으로 쓰이는 이미지, 색상, 폰트, 크기값 등은 enum + static, strut + static을 사용하여 관리합니다.
- (여담: Apple의 Combine framework 의 namespacing에 enum을 사용한다고 합니다.)
```swift
// enum + static
enum Font: String {
    case Regular = "myFont-Regular"
    case Bold = "myFont-Bold"

    func of(size: CGFloat) -> UIFont {
        return UIFont(name: self.rawValue, size: size)!
    }

    static func regular(size: CGFloat) -> UIFont {
        return Font.Regular.of(size: size)
    }

    static func bold(size: CGFloat) -> UIFont {
		    return Font.Bold.of(size: size)
    }
}

// Usage
Font.regular(size: 12)
Font.bold(size: 16)
```

```swift
// struct + static
struct Metric {
	static let buttonHeight: CGFloat = 35
}

// Usage
Metric.buttonHeight
```


