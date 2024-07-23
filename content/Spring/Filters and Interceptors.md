
![[Screenshot 2024-07-15 at 8.16.28 PM.png]]

**The key takeaway is that with _Filter_s, we can manipulate requests before they reach our controllers and outside of Spring MVC.** Otherwise, _HandlerInterceptor_s are a great place for application-specific cross-cutting concerns. By providing access to the target _Handler_ and _ModelAndView_ objects, we have more fine-grained control.