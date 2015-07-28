---
layout: post
title:  "Advanced NSOperation"
date:   2015-07-28 14:30:00+1000
categories: ios
tags: ios
---

NSOperation and NSOperationQueue look simple. They usually are used to do some background operations. While they are very powerful, actually they can be used to do authentications, play video and show alerts. Furthermore, we can use NSOperation dependency to organize the whole logic in apps. This is an interesting new design pattern that Apple encourages.

#### Serial Queues
Setting `NSOperationQueue` object `maxConcurrentOperationCount = 1` can turn this queue into a serial queue. Reducing the number of concurrent operations does not affect any operations that are currently executing. NSOperationQueueDefaultMaxConcurrentOperationCount is the value of the maximum number of operations based on system conditions.


#### NSOperation dependency
NSOperation dependency API is very straightforward. You can add or remove dependencies for an operation using the `addDependency:` or `removeDependency:` method. 
{% highlight swift %}
/*
    This operation is made of three child operations:
    1. The operation to download the JSON feed
    2. The operation to parse the JSON feed and insert the elements into the Core Data store
    3. The operation to invoke the completion handler
*/
downloadOperation = DownloadEarthquakesOperation(cacheFile: cacheFile)
parseOperation = ParseEarthquakesOperation(cacheFile: cacheFile, context: context)

let finishOperation = NSBlockOperation(block: completionHandler)

// These operations must be executed in order
parseOperation.addDependency(downloadOperation)
finishOperation.addDependency(parseOperation)
{% endhighlight %}
Operation dependency is very useful for complicated operations. Operation dependencies are guaranteed. Based on this, all business logic in apps can be put into NSOperation and these operations are connected by dependencies. Actually WWDC 2015 app was built based on NSOperation dependencies.

<img src="/images/nsoperation/wwdc_app_operation_dependency.png" alt="Drawing" style="width: 997px;height: 529px"/>

The below is the example of using NSOperation to open a new view controller, which is from Apple's advanced NSOperation demo code. One thing needed to notice is that this operation is set not to execute concurrently as this is a synchronous operation. From this example, we can see that the main business logic is put in the operation.

{% highlight swift %}
override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {
  if indexPath.section == 1 && indexPath.row == 0 {
      // The user has tapped the "More Information" button.
      if let link = earthquake?.webLink, url = NSURL(string: link) {
          // If we have a link, present the "More Information" dialog.
          let moreInformation = MoreInformationOperation(URL:
          queue?.addOperation(moreInformation)
      }
      else {
          // No link; present an alert.
          let alert = AlertOperation()
          alert.title = "No Information"
          alert.message = "No other information is available for this earthquake"
          queue?.addOperation(alert)
      }
  }
  
  tableView.deselectRowAtIndexPath(indexPath, animated: true)
}

/// An `Operation` to display an `NSURL` in an app-modal `SFSafariViewController`.
class MoreInformationOperation: Operation {
    // MARK: Properties

    let URL: NSURL
    
    // MARK: Initialization
    
    init(URL: NSURL) {
        self.URL = URL

        super.init()
        
        addCondition(MutuallyExclusive<UIViewController>())
    }
    
    // MARK: Overrides
 
    override func execute() {
        dispatch_async(dispatch_get_main_queue()) {
            self.showSafariViewController()
        }
    }
    
    private func showSafariViewController() {
        if let context = UIApplication.sharedApplication().keyWindow?.rootViewController {
            let safari = SFSafariViewController(URL: URL, entersReaderIfAvailable: false)
            safari.delegate = self
            context.presentViewController(safari, animated: true, completion: nil)
        }
        else {
            finish()
        }
    }
}

extension MoreInformationOperation: SFSafariViewControllerDelegate {
    func safariViewControllerDidFinish(controller: SFSafariViewController) {
        controller.dismissViewControllerAnimated(true) {
            self.finish()
        }
    }
}
{% endhighlight %}    