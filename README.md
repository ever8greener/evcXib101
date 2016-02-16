
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

`Remove` the Main.storyboard file

1.ProjectName-Info.plist 갱신 - `Remove` the Main storyboard base file name key.

1.nib file생성후   project’s view controller 와 연결함

 A. Create a nib file (File –> New –> File –> View)

 B. Update the File's Owner's class to whatever the project’s view controller is called

 C. Link the File's Owner's view outlet to the view object in the nib file

1.App delegate 파일  변경하기

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
