# Making Weather-Like ScrollView

![Making%20Weather-Like%20ScrollView%20087f4ef0d24641e0b48967d68ab48e86/Weather-Like-ScrollView.gif](Making%20Weather-Like%20ScrollView%20087f4ef0d24641e0b48967d68ab48e86/Weather-Like-ScrollView.gif)

---

# Problems

The `UIScrollView`, surrounded by the `UIPageViewController`, behaves like the above as the user scroll it.

It seems very easy and clear to implement, but there were several things to think about it.

- First, how is the header is shrunk as the user scroll upward?
- Second, how can the content be prevent from being scrolled until the header is completely shrunk?
- Third, how can they detect touch events, and make the content to be scrolled although the header and footer seems to not be contained in the scroll view?

# Clue

- First of all, I made 'scroll view' like below code.

    ```swift
    class ViewController: UIViewController {
    	...
    	override func viewDidLoad() {
    		super.viewDidLoad()
    		...
    		scrollView.contentInset = UIEdgeInsets(top: contentStartingPoint, left: 0, bottom: footerHeight, right: 0)
    		scrollView.contentInsetAdjustmentBehavior = .never
    		...
    	}
    }
    ```

    If I didn't set `contentInset` properly, the header would cover the content area.

- I used the delegate method, named `scrollViewDidScroll(_ scrollView: UIScrollView)`, for solving the first one.

    In this method, I calculated how far did the user scroll, then adjusted the header's height value.

- Actually the first solution solved the second one as soon as I solved that. I'll find the reason.
- To solve the last problem, I found one clue making the scroll view as the container (not the root view).

    ![Making%20Weather-Like%20ScrollView%20087f4ef0d24641e0b48967d68ab48e86/View_Hierarchy.001.png](Making%20Weather-Like%20ScrollView%20087f4ef0d24641e0b48967d68ab48e86/View_Hierarchy.001.png)

    I added the scroll view into the root as the lowest child, then set the rest's `zPosition` to some higher value. 

    So, I was able to interact with the scroll view, although I touched the header or footer area.

    But as you can see, the original weather app contains several buttons in the footer. I haven't solved it yet.
