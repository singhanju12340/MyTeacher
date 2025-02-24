---
Creation Time: Sunday, February 16th 2025
Modified Time: Monday, February 17th 2025
---
Its a feature in spring boot which allows pre processing such as validation, transformation and data modification on controllers and rest controllers in a centralised ways.


### Uses cases of Controller advice
1. Global exception handling: @ExceptionHandler within a @ControllerAdvice class to handle exceptions thrown by any controller,
2. Global Data Binding and Formatting: - @InitBinder methods to configure data binding settings (e.g., custom editors or formatters) for all controllers.
3. Model Attribute Population: With @ModelAttribute methods, you can add attributes to the model for all controllers, which is useful for populating common data (like user info or site-wide settings).
4. 