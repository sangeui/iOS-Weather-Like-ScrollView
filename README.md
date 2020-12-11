# Making Weather-Like ScrollView
![Weather-Like-ScrollView](https://user-images.githubusercontent.com/34618339/101879923-6d22c480-3bd5-11eb-8843-c8ed8ba69901.gif)

---

- 유저가 빠르게 스크롤 할 때, 헤더의 높이를 조정하는 부분에서 매끄럽지 않은 부분이 발생하.
- 헤더의 폰트 크기 결정하기
- 푸터에는 버튼이 들어가야 하는데, 터치 이벤트가 전달이 되는지 아직 확인하지 않음

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

![View Hierarchy 001](https://user-images.githubusercontent.com/34618339/101880011-93e0fb00-3bd5-11eb-9bf5-9f1320a7cdf3.png)


I added the scroll view into the root as the lowest child, then set the rest's `zPosition` to some higher value. 

   So, I was able to interact with the scroll view, although I touched the header or footer area.

   But as you can see, the original weather app contains several buttons in the footer. I haven't solved it yet.
