
interface builder 101

##Xcode7 에서 Empty 프로젝트만들기 ( 방법1 )
참고 [http://codefromabove.com/2014/09/xcode-6-removing-storyboards-and-creating-useful-empty-projects/]

1. 싱글뷰 프로젝트 만들고

1. ViewController.h   / ViewController.m  / Main.storyboard / LaunchScreen.xib `삭제`

1. info.plist 에서 main storyboard base file name 부분 `삭제`해버림.
이제 실행해봄.
`....rootViewController 어쩌고 저쩌고` Erro 발생함. 이는 정상임.
자..다음순서..

1. file-New - 코코아 클래스 만들기 이름은 ViewController
 이때, ` UIViewController 서브클래스로하고` 또  ` “Also create XIB File” 체크하기`

1. ViewController.h 수정
```objc
@class ViewController;
@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (strong, nonatomic) UIWindow *window;
@property (strong, nonatomic) ViewController *viewController;
```

그리고  ViewController.m 의 didFinishLaunchingWithOptions 수정
```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions{
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    self.viewController = [[ViewController alloc]initWithNibName:@"ViewController" bundle:nil];
    self.window.rootViewController = self.viewController;
    [self.window makeKeyAndVisible];
    return YES;
}
```
앗..근데 또 에러날것이다. m 파일의 헤더를 import  해주자

```objc
#import "AppDelegate.h"
#import "ViewController.h"
```

끝입니다.!
감사합니다! 

참고: info.plist 에서 LaunchScreen.xib 를 지우면 화면에서 상하 black 의 여백이 생김...따라서 안 지우는게 좋은듯..왜? 그건 모르겠습니다.

## Xcode7 에서 Empty 프로젝트만들기  (방법2입니다. 펌..) 이 방법이 더 간단합니다.
참고: [http://stackoverflow.com/questions/25783282/how-to-create-an-empty-application-in-xcode-6-without-storyboard]

1. the Main.storyboard file **삭제**하고

1. ProjectName-Info.plist 갱신 -  the Main storyboard base file name key도 **삭제**.

1. nib file생성후   project’s view controller 와 연결함

 A. Create a nib file (File –> New –> File –> View)

 B. Update the File's Owner's class to whatever the project’s view controller is called

 C. Link the File's Owner's view outlet to the view object in the nib file

1. App delegate 파일  변경하기

 A. 헤더 임포트 ` #import "ViewController.h" `
 
 B.  application:didFinishLaunchingWithOptions: method: 수정합니다. 


```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    MyViewController *viewController = [[MyViewController alloc] initWithNibName:@"MyViewController" bundle:nil];
    self.window.rootViewController = viewController;
    [self.window makeKeyAndVisible];
    return YES;
}
```
이상입니다.

## 두번째 뷰컨트롤러로 이동하기(짜잔~)

** 참고로 편의상  ViewController 클래스명칭을 여기부터는 MainViewController 로 수정했다.( 첫 진입 ViewController 라서 그렇게 지었다 )

 1.우선 navigation Controller 를 붙여야 한다. 이렇게하려면 AppDelegate.m 의 didFinishLaunchingWithOptions: 메소드 부분을 아래와 같이 다시 수정한다.
```objc
UINavigationController *navController = [[UINavigationController alloc]initWithRootViewController:viewController];
```
 2.rootViewController 를 navController 로 지정한다
```objc
self.window.rootViewController = navController;
```
 3.즉 didFinishLaunchingWithOptions: 메소드의 전체코드는 아래와 같다.
```objc
self.window = [[UIWindow alloc]  initWithFrame:[[UIScreen mainScreen]bounds]];
MainViewController *viewController = [[MainViewController alloc]initWithNibName:@"MainViewController" bundle:nil];

UINavigationController *navController = [[UINavigationController alloc]initWithRootViewController:viewController];
self.window.rootViewController = navController;

[self.window makeKeyAndVisible];
return YES;
```
 4.이 시점에서 실행해보면 그냥 empty project에서 navigation bar 영역이 상단에 나타난다.
 
 5.그리고 SecoundViewController.h / .m / .xib 3쌍의 파일을 만들어 준다. 왜냐면 가야할 곳을 만들어야하지않나?
 
ViewController.m에 버튼하나 올리고 SecondViewController 로 가는 IBAction 하나 코딩하고 xib 위의 버튼과 연결한다 (ctrl+drag).
```objc
- (IBAction)actionGoSecond:(UIButton *)sender {
    SecondViewController *secVC = [[SecondViewController alloc]initWithNibName:@"SecondViewController" bundle:nil];
    [self.navigationController pushViewController:secVC animated:YES];
}
```

아주아주 참고사항인데 swift 경우 아래와 같다.
```swift

func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: NSDictionary?) -> Bool {
    
    self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
    
    let navigat = UINavigationController()
    let vcw = ViewController(nibName: "ViewController", bundle: nil)
    
    navigat.pushViewController(vcw, animated: false)
    self.window!.rootViewController = navigat
    
    self.window!.makeKeyAndVisible()
    return true
    
}

//in ViewController.swift

@IBAction func GoToNext(sender : AnyObject)
{
    let ViewController2 = ViewController2(nibName: "ViewController2", bundle: nil)
    self.navigationController.pushViewController(ViewController2, animated: true)
}

```
## 세번째 뷰컨트롤러로 이동하기(짜짜잔 ~)

* 별로 다른 것은 없고, ThirdViewController 쌍을 만들어줌(.m .h .xib)
* SecondViewController ---> ThirdViewController 로 이동해야하므로...
* SecondViewController 에다가 아래 코딩해줌
```objc
- (IBAction)actionGoNext:(UIButton *)sender {
    ThirdViewController *nextVC = [[ThirdViewController alloc]initWithNibName:@"ThirdViewController" bundle:nil];
    [self.navigationController pushViewController:nextVC animated:YES];
}
```
## Tip / 이전,이후,루트 뷰로 이동(push,pop, popToRoot 시키기) 
UINavigation  계층에서 `Back` button 이 아닌 직접 올린 uibutton 에서도 이전 뷰로 이동가능하다.
 
* 새로운 뷰로 이동하기/빠져 나오기 
```objc
// 새로운 뷰 삽입하기
[navController pushViewController:newViewController animated:YES];
 
// 뷰컨트롤러 안에서 – 자기 자신을 네비게이션 컨트롤러에서 제거( = 이전 뷰 컨트롤러로 이동)
[self.navigationController popViewControllerAnimated:YES];
 
// 어디서든지 네비게이션 컨트롤러에 접근 가능할 때
[navController popViewControllerAnimated:YES];[/code]
```

* 최상위 뷰로 한번에 이동하기- Navigation화면상 이미 수차례 이동한 상태라면(이를테면 5번째) 한번에 최상위로 빠져나갈 수 있음.
```objc
[self.navigationController popToRootViewControllerAnimated:YES];[/code]
```
* 모달(Modal)뷰로 즉 full screen 으로 띄우기 (deprecated됨 in iOS6)
네비게이션 컨트롤러를 사용하되, 별개의 페이지처럼 띄우고 싶을 경우임(네비게이션바조차 나오지 않는 풀스크린의 뷰) 
```objc
// 모달 뷰 띄우기
[self.navigationController presentModalViewController:modelViewController animated:YES];
 
// 모달 뷰 제거 – 모달 뷰 컨트롤러 내부에서
[self dismissModalViewControllerAnimated:YES];[/code]
``` 
끝.





