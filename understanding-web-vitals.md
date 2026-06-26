CONCEPTS:
    1. What are web vitals vs core web vitals:
        Web vitals are a collection of metrrics that measure how well a website performs fromm a user's perspective. Where as core web vitals are the metrics that every website should focus to optimize inorder to provide a clean, optimized and faster loadingn and experience.
    2. FCP (First Contentful Paint) :
        FCP or First Contentful Paint is the metric which measures the time it takes to load the first pixel into the blank screen.  Measures the moment a blank screen is not blank anymore. It's good threshold is <=1.8 seconds. Common causes of poor FCP are: Large HTML files which takes a long time to parse and render,Render blocking CSS/JS files etc
    3. LCP (Largest Contentful Paint):
        LCP or Largest Contentful Paint is the metric which measures the time it takes to load the main content of the page. It maybe an image or a big content. It's good threshold is <=2.5 seconds. The common causes of poor LCP includes: Unoptimised images,slow server responses, Render block by CSS OR JS etcc
    4. TBT (Total Blocking Time):
        TBT or Total Blocking Time is the metric which measures the time the website has been unresponsive to the user due to a large JS script running in the background blocking all input to the browser. It's good threshold is <= 200ms. The common causes of TBT include Long JS tasks which takes time to complete, Heavy bundling of JS codes, Third-party scripts etc
    5. CLS (Cumulative Layout Shift):
        CLS or Cumulative Layout Shift is the metric which measures how much a page is trying to move around while a user is trying to interact with it. It's threshold is measured in decimals and it's good threshold value is <= 0.1. Common causes of poor CLS includes ads loading which doesn't have reserved space, cusstom font loading, Images without dimensions etc
    6. SI (Speed Index):
        SI or Speed Index is a metric similar to FCP but it measures the time it takes for all the pixels to load. It basically is the time it takes for the page to visually load. It's good threshold is <= 3.4 seconds. Common causes of poor SI include Large HTML files which takes a long time to parse and render,Render blocking CSS/JS files etc.
    7. Lab data (Lighthouse) vs Field data (CrUX/RUM):
        Lab data or Lighthouse as mentioned in the devtools are the metric produced when the nrowser audit on a standard device and network condition. Field data is the real perfomance data collected by a website from its own visitors. Lab data is tested in controlled environment where as field data is collected from real users. Lab data is good for debugging where as the field data is good for measuring actual user experience.

PRACTICAL INVESTIGATION:
| Site                         | Device  | FCP   | LCP   | TBT   | CLS  | SI   | Performance Score| Accessibility| Best Practices| SEO |
|------------------------------|---------|-------|-------|-------|------|------|------------------|--------------|---------------|-----|
| https://www.fleetstudio.com/ | Mobile  | 3.6 s | 9.1 s | 20 ms |  0   | 5.6 s|         64       |       83     |        96     |  82 |
| https://www.fleetstudio.com/ | Desktop | 1.9 s | 3.3 s | 0 ms  |  0   | 2.4 s|         70       |       83     |        100    |  82 |
| https://www.bbc.com          | Mobile  | 2.6 s | 6.1 s | 870 ms|  0   | 3.1 s|         53       |       100    |        96     |  100|
| https://www.bbc.com          | Desktop | 0.9 s | 2.1 s | 330 ms| 0.146| 4.8 s|         59       |       95     |        96     |  100|

### TOP ISSUES:

### fleetstudio.com - Mobile
1. Render-blocking requests (Est savings of 1,540 ms)
2. Use efficient cache lifetimes (Est savings of 661 KiB)
3. Improve image delivery (Est savings of 201 KiB)

### fleetstudion.com - Desktop:
1. Render-blocking requests (Est savings of 770 ms)
2. Use efficient cache lifetimes (Est savings of 697 KiB)
3. Improve image delivery (Est savings of 359 KiB)



### bbc.com - Mobile:
1. Use efficient cache lifetimes (Est savings of 252 KiB)
2. Font display (Est savings of 690 ms)
3. Improve image delivery (Est savings of 86 KiB)
### bbc.com - Desktop:
1. Use efficient cache lifetimes Est savings of 206 KiB
2. Legacy JavaScript Est savings of 64 KiB
3. Font display Est savings of 30 ms
### ROOT-CAUSE ANALYSIS
### fleetstudio.com: 
** FCP (Mobile 3.6s, Desktop 1.9s) - Poor **
-> Reduce unused CSS,Reduce unused JavaScript and defer loading scripts until they are required to decrease bytes consumed by network activity
** LCP (Mobile 9.1s, Desktop 3.3s) - Poor **
-> Optimize LCP by making the LCP image discoverable from the HTML immediately, and avoiding lazy-loading

** TBT (Mobile 20ms, Desktop 0ms) - Good **
-> Minimal JavaScript. Scripts are deferred, not blocking user interactions.

** CLS (Mobile 0, Desktop 0) - Good **
-> Page layout is stable. Ad spaces are reserved, fonts are stable. No unexpected shifts.

** SI (Mobile 5.6s, Desktop 2.4s) - Need Improvement from mobile side **
-> Page takes a while to load, Maybe rendering is slow.

### bbc.com
** FCP (Mobile 2.6s, Desktop 0.9s) - Acceptable but need improvement **
-> The page takes a while to load the first pixel maybe due to the HTML file

** LCP (Mobile 6.1s, Desktop 2.1s) - Poor perfomance (from mobile side) **
-> Optimize LCP by making the LCP image discoverable from the HTML immediately, reduce the dwnld time of images.

** TBT (Mobile 870ms, Desktop 330ms) - poor perfomance from mobile ** 
-> Requests are blocking the page's initial render,

** CLS (Mobile 0, Desktop 0.146) - Good **
-> Page layout is stable

** SI (Mobile 3.1s, Desktop 4.8s) - Acceptable **
-> Page takes a while to fully render

### PRIORITIZED RECCOMANDATIOINS:
### fleetstudio.com
1. Fix render-blocking CSS/JS first. This is the big one — 1.5s of savings on mobile, 770ms on desktop. It's basically why FCP is slow and why LCP is so bad on mobile (9.1s). Defer what you can, inline only the critical CSS.
2. Compress and resize the main image. Once render-blocking is sorted, the image itself is the next bottleneck for LCP. Switch to WebP, preload it.
3. Set longer cache headers. Cheap to do, but it only helps returning visitors — won't move today's scores since Lighthouse simulates a first-time visit.

### bbc.com
1. Add font-display: swap. One line of CSS, saves 690ms on mobile FCP. Easiest win on the list.
2. Compress images. Mobile LCP is at 6.1s ("Poor"), so trimming image weight helps.
3. Cache headers. Same deal as above — good for repeat visitors, not for the lab score.
4. Drop legacy JS polyfills (desktop). Smaller win, but reduces parse time a bit.

### FIRST RECCOMANDATIONS:
1. For fleetstudio, fix render blocking 
2. For bbc, add font-display
