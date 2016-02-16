
interface builder 101

##Xcode에서 Empty 프로젝트만들기
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
 self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    self.viewController = [[ViewController alloc] initWithNibName:@"ViewController" bundle:nil];
    self.window.rootViewController = self.viewController;
    [self.window makeKeyAndVisible];
    return YES;
```
앗..근데 또 에러날것이다. m 파일의 헤더를 import  해주자

```objc
#import "AppDelegate.h"
#import "ViewController.h"
```

 
