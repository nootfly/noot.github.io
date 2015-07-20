---
layout: post
title:  "ResearchKit study notes"
date:   2015-05-21 10:50:00+1000
categories: ios
tags: ios
---

We can know some coding practices of Apple when reading [ResearchKit](https://github.com/ResearchKit/ResearchKit) source code. 

#### 1. Use layout debug and layout test

{% highlight objective c lineanchors %}
#if LAYOUT_DEBUG
  self.backgroundColor = [[UIColor blueColor] colorWithAlphaComponent:0.2];
  _titleLabel.backgroundColor = [[UIColor greenColor] colorWithAlphaComponent:0.2];
  _valueLabel.backgroundColor = [[UIColor greenColor] colorWithAlphaComponent:0.2];
#endif

#if LAYOUT_TEST
  self.timeLeft = 60*5;
  self.hasHeartRate = YES;
  self.hasDistance = YES;
  self.distanceInMeters = 100;
  self.heartRate = @"22";
#endif
{% endhighlight %}    

#### 2. Storyboard VS code
in ResearchKit, all views are written by code, but in [AppCore](https://github.com/ResearchKit/AppCore), some views and view controllers are prepared in nibs or storyboards.

#### 3. Small classes
They create many small classes in one file, even these classes are similar.

{% highlight objective c lineanchors %}
    @interface APCTableViewDashboardProgressItem : APCTableViewDashboardItem

    @property (nonatomic) CGFloat progress;
    
    @end
    
    @interface APCTableViewDashboardGraphItem : APCTableViewDashboardItem
    
    @property (nonatomic, strong) APCScoring *graphData;
    
    @property (nonatomic) APCDashboardGraphType graphType;
    
    @property (nonatomic, strong) UIImage *minimumImage;
    
    @property (nonatomic, strong) UIImage *maximumImage;
    
    @property (nonatomic, strong) UIImage *averageImage;
    
    @end
    
    @interface APCTableViewDashboardMessageItem : APCTableViewDashboardItem
    
    @property (nonatomic) APCDashboardMessageType messageType;
    
    @end
{% endhighlight %} 

#### 4. Add Nullability annotations 
nullable, nonnull, NS_ASSUME_NONNULL_BEGIN and NS_ASSUME_NONNULL_END are used in the framework. So it can support Swift better.

#### 5. Use C functions as global functions
In ORKHelpers.h

    void ORKEnableAutoLayoutForViews(NSArray *views);

In ORKHelpers.m
{% highlight objective c lineanchors %}
    void ORKEnableAutoLayoutForViews(NSArray *views) {
    [views enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
        [(UIView *)obj setTranslatesAutoresizingMaskIntoConstraints:NO];
    }];
    }
{% endhighlight %} 
#### 6. Add UIContentSizeCategoryDidChangeNotification to labels and text fields

    - (void)init_ORKAnswerTextField {
      [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(updateAppearance)
                                                 name:UIContentSizeCategoryDidChangeNotification
                                               object:nil];
       [self updateAppearance];
    }

    - (void)updateAppearance {
        self.font = [[self class] defaultFont];
        [self invalidateIntrinsicContentSize];
    }

#### 7. System design
ResearchKit is a pure framework which provides all basic elements, while [AppCore](https://github.com/ResearchKit/AppCore) is a layer built on top of ResearchKit. AppCore builds basic layouts and basic functions for an app, such as timer, status restore, core data, JSON serialization and network. So it's easy to create a ResearchKit app based on AppCore. This is an interesting way to organize codes. AppCore is just like a template app. When creating a new app, we just add some customization to AppCore code base.

#### 8. Other interesting things
1) NSDictionaryOfVariableBindings is a useful macro to combine keys and values together.

    NSDictionary *dictionary = NSDictionaryOfVariableBindings(_countDownLabel, _startTimerButton);

2) __unused macro is used to avoid a warning.

    - (UIImageView *)imageView {
     __unused UIView *view = [self view];
     return _activeStepView.imageView;
    }

3) Using NSProcessInfo to check iOS version

    if ([[NSProcessInfo processInfo] 
      isOperatingSystemAtLeastVersion:(NSOperatingSystemVersion){8, 2, 0}]) {
            [[NSNotificationCenter defaultCenter] addObserver:self
                                  selector:@selector(healthKitUserPreferencesDidChange:)
                                  name:HKUserPreferencesDidChangeNotification
                                 object:healthStore];
    }

4) Play system sound

    - (void)playSound {
     if (_alertSoundURL == nil) {
        _alertSoundURL = [NSURL URLWithString:@"/System/Library/Audio/UISounds/short_low_high.caf"];
        AudioServicesCreateSystemSoundID((__bridge CFURLRef)(_alertSoundURL), &_alertSound);
     }
     AudioServicesPlaySystemSound(_alertSound);
    }

    - (void)dealloc {
     AudioServicesDisposeSystemSoundID(_alertSound);
    }

5) Use NS_DESIGNATED_INITIALIZER to trigger a warning

[This article](http://useyourloaf.com/blog/2014/08/19/xcode-6-objective-c-modernization.html) explains Xcode 6 Objective-C modernization tool.

    - (instancetype)initWithIdentifier:(NSString *)identifier
                  recorderSettings:(nullable NSDictionary *)recorderSettings
                              step:(nullable ORKStep *)step
                   outputDirectory:(nullable NSURL *)outputDirectory 
                   NS_DESIGNATED_INITIALIZER;

6) Use Visibility Attributes to export classes, functions

    #import <ResearchKit/ResearchKit_Private.h>

    NS_ASSUME_NONNULL_BEGIN
    
    ORK_CLASS_AVAILABLE
    @interface ORKAudioStepViewController : ORKActiveStepViewController
    
    @end
    
    NS_ASSUME_NONNULL_END

    #define ORK_CLASS_AVAILABLE __attribute__((visibility("default")))
