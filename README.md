# KeenCodeStyleGuide

## 참고
- [Apple API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/#naming)
- [토미의 개발노트 - 팀에서 사용 중인 Swift Style Guide](https://jusung.github.io/Swift-Code-Convention/)
- [스타일쉐어 컨벤션](https://github.com/StyleShare/swift-style-guide)
- [에어비엔비 컨벤션](https://github.com/airbnb/swift)


----

## 목차
1. 코드 포메팅
	- 1-1) import
	- 1-2) 작성 순서
	- 1-3) 빈줄
	- 1-4) 들여쓰기
	- 1-5) 띄어쓰기
	- 1-6) 반복적으로 사용되는 컴포넌트 값

2. 네이밍
	- 2-1) 오브젝트
	- 2-2) 선언
	- 2-3) ViewController
	- 2-4) NavigationController

3. 코드 스타일
	- 3-1) 조건문
	- 3-2) 함수
	- 3-3) 주석
	- 3-4) Eror Handling - if let, guard let
	- 3-5) Switch문
	- 3-6) Closure
	- 3-7) Optional 처리
	- 3-8) Class
	- 3-9) View Life-Cycle
	- 3-10) 프로토콜 Extension
	- 3-11) 커스텀 Font 사용 시
	- 3-12) Static Class 사용 시
	- 3-13) CGFloat 값의 소수점이 0인 경우

4. Snapkit
	- 4-1) Left/Right vs Leading/Trailing

----

## 1. 코드 포메팅

### 1-1) import
- 알파벳 순으로 정렬합니다.
- 해당 파일에서 필요로 하는 프레임워크/라이브러리는 모두 작성합니다.


</br>

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

</br>

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

</br>

### 1-4) 들여쓰기
- 탭 간격은 4space 로 합니다.
- 들여쓰기는 Xcode에서 제공하는 ^ + i 를 눌렀을 때, 적용되는 space를 사용합니다.

</br>

### 1-5) 띄어쓰기
- 콜론(:)을 사용할 때는 콜론의 오른쪽에만 공백을 둡니다.
- 삼항 연산자의 경우엔 앞뒤로 띄웁니다.
- 콤마(,) 뒤에는 공백을 둡니다.
```swift
let number: Int = 3
let isOdd: Bool = number % 2 == 1 ? true : false
let lotto: Int = [1, 10, 32, 40, 22, 12]
```

</br>

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

</br>
</br>

----

## 2. 네이밍

### 2-1) 오브젝트
- `붙여주고자 하는 이름` + `오브젝트 타입` 형식으로 작성합니다.
- 이름을 과도학 ㅔ축약하지 않습니다. (ex. button -> btn)
```swift
✅ Preferred
@IBOutlet weak var searchIdButton: UIButton!
@IBOutlet weak var passwordTextField: UITextField!

⛔️ Not Preferred
@IBOutlet weak var searchIdBT: UIButton!
@IBOutlet weak var textPasswd: UITextField!
```

- `Bool` 타입의 변수는 비교하는 값일 땐 `is` 존재하는지 확인하는 값일 땐 `has`를 사용합니다.
```swift
✅ Example
var animal = "dog"
var isCat: Bool

animal = "cat"
isCat = (animal == "cat")

✅ Example
var animals = ["dog", "rabbit", "bird", "pig", "horse"]
var hasCat: Bool

animals.append("cat")
hasCat = animal.contains("cat")
```

- 버튼액션 `sender`는 `Any`가 아닌 명확하게 명시해줍니다.
```swift
✅ Preferred
@IBAction func onNext(_ sender: UIButton) {
	// some action..
}

⛔️ Not Preferred
@IBAction func onNext(_ sender: Any) {
	// some action..
}
```

</br>

### 2-2) 선언
- 최초 선언 시에는 항상 `타입명`을 함께 작성합니다.
- 특별한 이유가 없다면 `변수`보다는 `상수`를 먼저 작성합니다.
- `Lower Camel Case`로 작성합니다.
- 단 클래스, 구조체, 열거형은 `Upper Camel Case`로 작성합니다..
	- 열거형의 `case`문은 `Lower Camel Case`로 작성합니다.
```swift
✅ Preferred
class Company {
	let name: String = "herren"
	var age: Int = 6
}

⛔️ Not Preferred
class company {
	let name:String = "herren"
	var age = 6
}
```

- 인스턴스를 선언 시에는 타입을 적지 않습니다.
	- 인스턴스에 타입이 적혀있기 때문에 중복 작성하지 않습니다.
```swift
✅ Preferred
var infoList = [SomeInfo]()

⛔️ Not Preferred
var infoList: [SomeInfo] = [SomeInfo]()
```

</br>

### 2-3) ViewController
- ViewController 변수/상수 선언시에는 VC로 줄여서 네이밍을 합니다.
	- ex) `let mainVC = mainViewController()`
- storyboard에서 storyboardID 즉, withIdentifier에도 VC네이밍을 사용합니다.
	- ex) `storyvoardID` -> `mainVC`
```swift
let mainVC = UIStoryboard(name: "User", bundle: nil).instantiateViewController(withIdentifier: "mainVC")
```

2-4) NavigationController
- NavigationController 변수/상수 선언시에는 NAV로 줄여서 네이밍을 합니다.

```swift
let mainNAV = UINavigationController(rootViewController: mainVC)
```

</br>
</br>

----

## 3. 코드 스타일

### 3-1) 조건문
- `else if`, `else`문은 코드블록(`{ }`) 과 함께 한 줄로 작성합니다.

```swift
if state == "A" {
	// some code..
} else if state == "B" {
	// some code..
} else {
	// some code..
}
```

- `else if` 로 분기 처리할 때는 `else`를 쓰지 않고 작성합니다.
```swift
state == "A" || "B" || "C" 일 때,

✅ Preferred
if state == "A" {
	// some code..
} else if state == "B" {
	// some code..
} else if state == "C"{
	// some code..
}

⛔️ Not Preferred
...
} else { // "C"
	// some code..
}
```

- `if` 문으로 `Bool` 타입의 변수를 식별할 때는 `!`를 사용합니다.
```swift
✅ Preferred
if isNumber {
	// true
}
if !isNumber {
	// false
}

⛔️ Not Preferred
if isNumber == true {
	// true
}
if isNumber == false {
	// false
}
```

</br>

### 3-2) 함수
- Lower Camel Case 로 작성합니다.
- 함수 안에 작성된 코드가 없다면 한 줄로 작성합니다.
```swift
✅ Preferred
func isEmpty(str: String) -> Bool { }

⛔️ Not Preferred
func isOdd(number:Int) -> Bool {

}
```

</br>

### 3-3) 주석
- [참고 링크](https://yoojin99.github.io/app/Swift-Documentation/)
- 주석은 함수, 클래스, 구조체, 변수 등 문서화 가능한 타입은 문서화 주석을 사용합니다.
- 간단한 한 줄 주석은 `///` 를 사용합니다.
```swift
/// hello를 프린트하는 함수
func printHello() {
	print("hello!")
}
```

- 여러줄의 자세한 설명이 필요한 함수는 `/** */` 를 사용합니다.
```swift
/**
    사람에 대한 모델

	- Parameters: 
		- name : 사람의 이름
    	- age : 사람의 나이
*/
struct Person {
    let name: String
    let age: Int
}
```

- 그 외의 조건문, 반복문 등 문서화가 불가능한 타입들에 대해선 기본 주석(`//`)을 사용합니다.
```swift
func printHello(name: String?) {
	if  let name = name { // name 인자가 nil 이 아닌 경우
		print("hello, \(name)!")
	} else { // name이 nil인 경우
		print("hello, stranger!")
	}
}

```

- 주석 시 [comment here](https://apps.apple.com/us/app/comment-here/id1406737173?mt=12) 플러그인을 사용합니다.
	- 해당 코드가 작성된 라인의 앞이 아니라 코드의 시작 부분에 주석 표시가 됩니다.
```swift
✅ Preferred
func someFunction() {
	for i in 0..<5 {
		// print("test\(i)")
		// print("next")
	}
}

⛔️ Not Preferred
func someFunction() {
	for i in 0..<5 {
//		print("test\(i)")
//		print("next")
	}
}
```

- 조건문의 주석은 같은 라인에 작성합니다.
```swift
✅ Preferred
if isNumber == true { // 숫자인지 확인
	// some code..
} else { // 숫자가 아님.
	// some code..
}

⛔️ Not Preferred
// 숫자인지 확인
if isNumber == true { 
	// some code..
// 숫자가 아님.
} else {
	// some code..
}
```

 - Pragma Mark는  `MARK`, `???`, `TODO`, `FIXME`, `-` 를 사용합니다.
	- `MARK` : 코드 영역을 구분할 때
	- `???` : 레거시 중 이해할 수 없거나 의문이 드는 코드에 코멘트를 작성할 때
	- `TODO` : 앞으로 구현해야하거나, 미완성 코드에 대한 코멘트를 작성할 때
	- `FIXME` : 테스트 용으로 작성헀거나 임시 코드를 작성할 때

- MARK 주석으로 코드를 구분합니다.
	- `Properties` : 기타 프로퍼티 관련
	- `UI` : UI 선언 관련
	- `View Life-Cycle` : 뷰 생명주기 관련
	- `Init` : 초기화 코드 관련
	- `Actions` : 액션 함수 관련
	- `Extensions` : class에 대한 extension 관련
    	- UITableViewDelegate
    	- UITableViewDataSource
	- `Setup Layout` : addSubView나 Snapkit을 사용한 제약조건 설정 관련
	- `Network` : API 호출 처리 관련
	- `ETC` : 기타 함수 관련

```swift
class SomeViewController: UIViewController {
	// MARK: - Properties

	var name: String

	// MARK: - UI

	var someView: CustomView

	// MARK: - View Life-Cycle

	override func viewDidLoad() {
		super.viewDidLoad()
		someView.tableView.delegate = self
		someView.tableView.dataSource = self

		someView.someButton.addTarget(self, action: #selector(onDone), for: .touchUpInside)
	}

	// MARK: - ETC
	...
	// MARK: - Actions

	@objc func onDone() {
		self.dismiss(animated: true)
	}
}

// MARK: Extensions
// MARK: - UITableViewDataSource

extension SomeViewController: UITableViewDataSource {
	...
}

// MARK: - UITableViewDelegate

extension SomeViewController: UITableViewDelegate {
	...
}
```

</br>

### 3-4) Error Handling - if let, gaurd let
- `if let`의 `if` 구문, `guard let`의 `guard` 구문에 여러 개의 상수를 선언하는 경우
	- `let` 마다 줄바꿈을 하여 아래의 코드와 같이 작성합니다.
- `guard` 문의 경우 `else` 구문에 `return` 밖에 없다면 한 줄로 작성합니다.

```swift
✅ Preferred
if 
  let user = self.veryLongFunctionNameWhichReturnOptionalUser(),
  let name = user.veryLongFunctionNameWhichReturnsOptionlName() {
		// some code..
 }

guard 
   let user = self.veryLongFunctionNameWhichReturnsOptionalUser(),
   let name = user.veryLongFunctionNameWhichReturnsOptionalName()
else {
   return
}

var someInfoFromOtherViewController: String?
guard let someInfo = someInfoFromOtherViewController else { return }

⛔️ Not Preferred
if let user = self.veryLongFunctionNameWhichReturnOptionalUser(), let name = user.veryLongFunctionNameWhichReturnsOptionlName() {
		// some code..
 }

guard let someInfo = someInfoFromOtherViewController else { 
	return
}
```

</br>

### 3-5) Switch 문
- `case` 문이 한 줄이 아닐 때는 케이스와 케이스 사이에 빈줄을 넣습니다.

```swift
enum Direction {
	case north
	case south
	case east
	case west
}

let direction: Direction =  .north

func someFunction() {
	// some code..
}

switch direction {
	case .north:
		print("북쪽으로 가십쇼")
		someFuntion()

	case .south: print("남쪽으로 가십쇼")
	case .east: print("동쪽으로 가십쇼")
	case .west: print("서쪽으로 가십쇼")
}
```
- 가능하다면 `default` 사용을 지양합니다.
```swift
✅ Preferred
switch anEnum {
case .a: print("a")
case .b, .c: print("b or c")
}

✅ 써야만 하는 경우
switch anEnum {
case .a: print("a")
default: break
}

⛔️ Not Preferred
switch anEnum {
case .a: print("a")
default: print("not a")
}
```

</br>

### 3-6) Closure
- 파라미터와 리턴 타입이 없는 클로저 정의 시에는 `() -> Void`를 사용합니다.
```swift
✅ Preferred
func fetchData(@escaping completion: () -> Void) {
		// some code..
}

⛔️ Not Preferred
func fetchData(@escaping completion: () -> ()) {
		// some code..
}
```

- 클로저 정의 시
	- 파라미터가 튜플타입이 아니라면, 괄호를 사용하지 않습니다.
	- `파라미터` + `in` 까지 작성한 후 줄바꿈을 합니다.
```swift
✅ Preferred
completion { data, error in
	print(data)
}

⛔️ Not Preferred
completion { (data, error) in
	print(data)
}
```

- 한 줄로도 충분한 클로저는 반드시 각 괄호 양쪽에 공백을 추가해야 합니다.
	- ex) `map`, `filter`, `reduce`와 같은 고차 함수
```swift
✅ Preferred
numbers.filter { $0 != 0 }.map { String($0) }

⛔️ Not Preferred
numbers
	.filter {$0 != 0}
	.map { String($0) }
```

- 가능한 경우 타입 정의를 생략합니다.
- 사용하지 않는 파랕미터는 와일드카드(`_`)를 사용해 표시합니다.
```swift
✅ Preferred
completion { _, _, three in
	print(three)
}

⛔️ Not Preferred
completion { (one, two, three) in
	print(three)
}
```

</br>

### 3-7) Optional 처리
- 가능하면 강제 언래핑(`!`)을 지양합니다.
	- gaurd let이나 if let을 활용합니다.
```swift
var userInfo: User?

✅ Preferred
func userInfo(user: User) -> String {
	if let user = userInfo {
		return "사용자명: \(user.name)"
	} else {
		return "사용자가 없습니다."
	}
}

⛔️ Not Preferred
func userInfo(user: User) -> String {
	return "사용자명: \(userInfo!.name)"
}
```

</br>

### 3-8) Class
- 초기화 코드 전에 상수/변수를 선언합니다.
- 상수/변수 선언 순서는 다음과 같습니다.
	- delegate
	- UI 관련
	- 기타 상수/변수
```swift
class CollectionListView: UIView {
    weak var delgate: CollectionListViewDelegate?
    
    var collectionView: UICollectionView!
    
    var someIndex: Int = 0
    
		override init(frame: CGRect) {
        super.init(frame: frame)
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```

- 더 이상 상속이 발생하지 않는 클래스는 항상 `final` 키워드로 선언합니다.
```swift
✅ Preferred
final class ViewController: UIViewController {
 // some code..
}

⛔️ Not Preferred
class ViewController: UIViewController {
 // some code..
}
```

- Class 선언 부 상단에 어떤 화면의 class인지 간략하게 적어줍니다.
	- 사용하지 않는 viewController는 따로 명시해줍니다.
```swift
/// 마이페이지 메인 화면
final class MyPageViewController: UIViewController {
 // some code..
}
```

</br>

### 3-9) View Life-Cycle (뷰 생명주기)
- 구문 내부 선언 순서는 다음과 같습니다.
	- delegate
	- 기타 초기 세팅 (UI, data ...)
	- add subview 관련
	- setup layout 관련
```swift
var name: String?

override func viewDidLoad() {
	super.viewDidLoad()

	tableView.delegate = self
	tableView.dataSource = self
	textField.delegate = self

	setLayout()

	if let name = name {
		textField.text = "\(name)님 환영합니다~!"
	} else {
		textField.text = "닉네임을 입력해주세요"
	}

	NotificationCenter.default.addObserver(self, selector: #selector(keybordWillDisapear(_:)), name: UIResponder.keyboardWillHideNotification, object: nil)
  NotificationCenter.default.addObserver(self, selector: #selector(keybordDidDisapear(_:)), name: UIResponder.keyboardDidHideNotification, object: nil)
}
    
func setLayout() {
	 // some code..
	/*
		- add subviews
		- set constraints
	*/
}
```

</br>

### 3-10) 프로토콜 extension
- 프로토콜을 준수할 때는 extension을 만들어 관련 메서드를 분리하여 작성합니다.
```swift
✅ Preferred
final class ViewController: UIViewController {
	// some code..
}

// MARK: - Extensions
// MARK: - UITableViewDataSource
extension ViewController: UITableViewDataSource {
	// ...
}

// MARK: - UITableViewDelegate
extension ViewController: UITableViewDelegate {
	// ...
}

⛔️ Not Preferred
final class ViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
	// some code..
}
```

</br>

### 3-11) Font 설정 방법
- 기본 폰트를 커스텀 폰트를 사용하는 경우, `UIFont Extension` 에 커스텀 폰트를 추가하여 사용합니다.
	- 스토리보드로 폰트를 지정하는 것을 지양합니다.
```swift
✅ Preferred
let label = UILabel()
label.font = .someCustomFont(ofSize: 14, weight: .regular)
```

</br>

### 3-12) Static Class 접근 방법
- `UIColor`, `UIFont`, `UIImage` 등 기본으로 제공되고 생략 가능한 `static class`에 접귾라 때는 해당 `prefix`를 생략합니다.
	- 커스텀한 모듈 클래스/구조체에 대해서는 생략하지 않습니다.
```swift
✅ Preferred
let label = UILabel()
label.font = .someCustomFont(ofSize: 14, weight: .regular)

⛔️ Not Preferred
let label = UILabel()
label.font = UIFont.someCustomFont(ofSize: 14, weight: .regular)
```

</br>

### 3-13) CGFloat 값의 소수점 자리가 0 인 경우
- 소수점이 있는 숫자의 경우, 소수점 자리 값이 `0`인 경우 `0`을 생략합니다.
```swift
✅ Preferred
let width = footerLabel.frame.size.width + 12

⛔️ Not Preferred
let width = footerLabel.frame.size.width + 12.0
```
- 만약 해당 타입의 유추가 모호한 경우에는 타입을 명시해줍니다.
```swift
✅ Preferred
let width: CGFloat = 12

⛔️ Not Preferred
let width = 12
```

</br>
</br>

----

## 4. Snapkit

### 4-1) Left/Right vs Leading/Traling
- Left/Right 보다는 Leading/Trailing 을 사용합니다., (Apple의 권장사항)
	- 하지만 경우에 따라, 특정 국가와 상관없이 동일한 UX를 위해 항상 똑같은 UI를 제공하고 싶은 경우에 한해 Left/Right를 사용합니다.
```swift
✅ Preferred
label.snp.make {
	$0.leading.trailing.equalToSuperView()
}

⛔️ Not Preferred
label.snp.make {
	$0.left.right.equalToSuperView()
}
```

