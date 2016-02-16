# evcXib101
interface builder 101

##Xcode에서 Empty 프로젝트만들기
 참고 [http://codefromabove.com/2014/09/xcode-6-removing-storyboards-and-creating-useful-empty-projects/]

1.싱글뷰 프로젝트 만들고
2. ViewController.h   / ViewController.m  / Main.storyboard / LaunchScreen.xib *삭제*
3. info.plist 에서 main storyboard base file name 부분 *삭제*해버림.

이제 실행해봄.
`....rootViewController 어쩌고 저쩌고` Erro 발생함. 이는 정상임.
자..다음순서..

4. file-New - 코코아 클래스 만들기 이름은 ViewController
 * UIViewController 서브클래스로하고
 * “Also create XIB File” 체크하기

5.ViewController.h 수정
```objc
@class ViewController;
@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (strong, nonatomic) UIWindow *window;
@property (strong, nonatomic) ViewController *viewController;
```
5.ViewController.m 의 didFinishLaunchingWithOptions 수정
```objc
 self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    self.viewController = [[ViewController alloc] initWithNibName:@"ViewController" bundle:nil];
    self.window.rootViewController = self.viewController;
    [self.window makeKeyAndVisible];
    return YES;
```
6.에러날것이다. m 파일의 헤더를 import  해주자
```objc
#import "AppDelegate.h"
#import "ViewController.h"
```

참고사이트: [http://codefromabove.com/2014/09/xcode-6-removing-storyboards-and-creating-useful-empty-projects/]
