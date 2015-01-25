---
layout: post
title:  "iOS Cookbook"
date:   2014-07-17 15:12:28+1000
categories: ios
tags: ios
---
This blog is to record useful codes for iOS development.

##### Capture current screen
    -(UIImage *)captureView:(UIView *)aView {
    CGRect screenRect = [aView bounds];
    UIGraphicsBeginImageContextWithOptions(screenRect.size, NO, 0.0);
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    //CGContextFillRect(ctx, screenRect);
    [aView.layer renderInContext:ctx];
    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    return newImage;
    }
    
#### Share Framework
     if ([SLComposeViewController isAvailableForServiceType:SLServiceTypeSinaWeibo]) {
        SLComposeViewController *mySLComposerSheet = [SLComposeViewController composeViewControllerForServiceType:SLServiceTypeSinaWeibo];
        NSString *text = NSLocalizedString(@"trend", "");
        [mySLComposerSheet setInitialText:text];
        [mySLComposerSheet addImage:[self captureView:self.view]];
        [mySLComposerSheet addURL:[NSURL URLWithString:@"https://itunes.apple.com/au/app/guangzhou-air-quality-index/id533417463?mt=8"]];
        [mySLComposerSheet setCompletionHandler:^(SLComposeViewControllerResult result) {
            switch (result) {
                case SLComposeViewControllerResultCancelled:
                    DLog(@"Post Canceled");
                    break;
                case SLComposeViewControllerResultDone:
                    DLog(@"Post Sucessful");
                    break;
                    
                default:
                    break;
            }
        }];
        [self presentViewController:mySLComposerSheet animated:YES completion:nil];
    } else {
        [SVProgressHUD showErrorWithStatus:NSLocalizedString(@"socialSetting", nil)];
    }