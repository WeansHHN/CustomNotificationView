# CustomNotificationView

**CustomNotificationView** là một view thông báo tùy chỉnh nhẹ cho iOS, cho phép bạn hiển thị thông báo với tiêu đề, nội dung và biểu tượng ứng dụng. View có thể tự động ẩn sau một khoảng thời gian và hỗ trợ thao tác vuốt để ẩn.

---

## Demo Notification
<img style="width: 500px" src="https://github.com/WeansHHN/CustomNotificationView/blob/main/video_2025-01-22_23-27-59.gif?raw=true">

## Tính năng nổi bật

- Hiển thị thông báo với biểu tượng ứng dụng, tiêu đề và nội dung.
- Tự động ẩn sau khoảng thời gian tùy chỉnh.
- Hỗ trợ vuốt lên để ẩn thủ công.
- Hiệu ứng hoạt họa mượt mà khi hiển thị và ẩn thông báo.

---

## Yêu cầu

- iOS 10.0 hoặc mới hơn.
- Dự án viết bằng Objective-C.

---

## Cài đặt

1. Thêm hai file `CustomNotificationView.h` và `CustomNotificationView.m` vào dự án của bạn.
2. Đảm bảo bạn import file `CustomNotificationView.h` ở nơi cần sử dụng thông báo.

---

## Hướng dẫn sử dụng

### 1. Import `CustomNotificationView`

Trong file mà bạn muốn hiển thị thông báo, hãy import header:

```objc
#import "CustomNotificationView.h"
```

### 2. Tạo và Hiển thị Thông Báo

Tạo một instance của `CustomNotificationView` và thiết lập:

```objc
CustomNotificationView *notification = [[CustomNotificationView alloc] initWithAppIcon:[UIImage imageNamed:@"AppIcon"]
                                                                                  title:@"Tiêu đề thông báo"
                                                                                message:@"Đây là nội dung thông báo tuỳ chỉnh."];
notification.dismissDelay = 5.0; // Tự động ẩn sau 5 giây
[notification showNotificationInView:[[UIApplication sharedApplication].keyWindow rootViewController].view];
```

### 3. Tùy chỉnh Thời gian Ẩn

Bạn có thể thiết lập thuộc tính `dismissDelay` để thay đổi thời gian thông báo hiển thị:

```objc
notification.dismissDelay = 10.0; // Tự động ẩn sau 10 giây
```

### 4. Vuốt Để Ẩn

Thông báo hỗ trợ thao tác vuốt lên để ẩn thủ công.

---

## Ví dụ Mã nguồn

### CustomNotificationView.h

```objc
#import <UIKit/UIKit.h>

@interface CustomNotificationView : UIView

@property (nonatomic, assign) NSTimeInterval dismissDelay;

- (instancetype)initWithAppIcon:(UIImage *)appIcon title:(NSString *)title message:(NSString *)message;
- (void)showNotificationInView:(UIView *)view;

@end
```

### CustomNotificationView.m

```objc
#import "CustomNotificationView.h"

@interface CustomNotificationView ()

@property (nonatomic, strong) UIImageView *iconImageView;
@property (nonatomic, strong) UILabel *titleLabel;
@property (nonatomic, strong) UILabel *messageLabel;

@end

@implementation CustomNotificationView

- (instancetype)initWithAppIcon:(UIImage *)appIcon title:(NSString *)title message:(NSString *)message {
    self = [super initWithFrame:CGRectZero];
    if (self) {
        [self setupViewWithAppIcon:appIcon title:title message:message];
        self.dismissDelay = 3.0; // Thời gian ẩn mặc định
    }
    return self;
}

- (void)setupViewWithAppIcon:(UIImage *)appIcon title:(NSString *)title message:(NSString *)message {
    self.backgroundColor = [[UIColor blackColor] colorWithAlphaComponent:0.8];
    self.layer.cornerRadius = 10;

    self.iconImageView = [[UIImageView alloc] initWithImage:appIcon];
    self.iconImageView.layer.cornerRadius = 8;
    self.iconImageView.clipsToBounds = YES;
    [self addSubview:self.iconImageView];

    self.titleLabel = [[UILabel alloc] init];
    self.titleLabel.text = title;
    self.titleLabel.font = [UIFont boldSystemFontOfSize:16];
    self.titleLabel.textColor = [UIColor whiteColor];
    [self addSubview:self.titleLabel];

    self.messageLabel = [[UILabel alloc] init];
    self.messageLabel.text = message;
    self.messageLabel.font = [UIFont systemFontOfSize:14];
    self.messageLabel.textColor = [UIColor lightGrayColor];
    self.messageLabel.numberOfLines = 2;
    self.messageLabel.lineBreakMode = NSLineBreakByWordWrapping;
    self.messageLabel.preferredMaxLayoutWidth = 200;
    [self addSubview:self.messageLabel];

    UISwipeGestureRecognizer *swipeUp = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(dismissNotification)];
    swipeUp.direction = UISwipeGestureRecognizerDirectionUp;
    [self addGestureRecognizer:swipeUp];

    [self setupConstraints];
}

- (void)setupConstraints {
    self.iconImageView.translatesAutoresizingMaskIntoConstraints = NO;
    self.titleLabel.translatesAutoresizingMaskIntoConstraints = NO;
    self.messageLabel.translatesAutoresizingMaskIntoConstraints = NO;

    [NSLayoutConstraint activateConstraints:@[
        [self.iconImageView.leadingAnchor constraintEqualToAnchor:self.leadingAnchor constant:10],
        [self.iconImageView.centerYAnchor constraintEqualToAnchor:self.centerYAnchor],
        [self.iconImageView.widthAnchor constraintEqualToConstant:40],
        [self.iconImageView.heightAnchor constraintEqualToConstant:40],
        [self.titleLabel.leadingAnchor constraintEqualToAnchor:self.iconImageView.trailingAnchor constant:10],
        [self.titleLabel.trailingAnchor constraintEqualToAnchor:self.trailingAnchor constant:-10],
        [self.titleLabel.topAnchor constraintEqualToAnchor:self.topAnchor constant:10],
        [self.messageLabel.leadingAnchor constraintEqualToAnchor:self.iconImageView.trailingAnchor constant:10],
        [self.messageLabel.trailingAnchor constraintEqualToAnchor:self.trailingAnchor constant:-10],
        [self.messageLabel.topAnchor constraintEqualToAnchor:self.titleLabel.bottomAnchor constant:5],
        [self.messageLabel.bottomAnchor constraintLessThanOrEqualToAnchor:self.bottomAnchor constant:-10]
    ]];
}

- (void)showNotificationInView:(UIView *)view {
    [view addSubview:self];
    UIWindow *keyWindow = [UIApplication sharedApplication].keyWindow;
    CGFloat notificationWidth = 300;
    CGFloat notificationHeight = self.frame.size.height;
    self.frame = CGRectMake(keyWindow.frame.size.width, 40, notificationWidth, notificationHeight);
    [UIView animateWithDuration:0.5 animations:^{
        self.frame = CGRectMake(keyWindow.frame.size.width - notificationWidth - 10, 20, notificationWidth, notificationHeight);
    } completion:^(BOOL finished) {
        [NSTimer scheduledTimerWithTimeInterval:self.dismissDelay target:self selector:@selector(dismissNotification) userInfo:nil repeats:NO];
    }];
}

- (void)dismissNotification {
    [UIView animateWithDuration:0.5 animations:^{
        self.frame = CGRectMake([UIScreen mainScreen].bounds.size.width, self.frame.origin.y, self.frame.size.width, self.frame.size.height);
    } completion:^(BOOL finished) {
        [self removeFromSuperview];
    }];
}

@end
```

---

## Ghi chú

Đây là một View hiện thông báo gọn, đẹp và mượt.

## Liên hệ

Nếu bạn có bất kỳ câu hỏi nào, vui lòng liên hệ:

- Website: https://haininh.site
- GitHub: [WeansHHN](https://github.com/WeansHHN)

