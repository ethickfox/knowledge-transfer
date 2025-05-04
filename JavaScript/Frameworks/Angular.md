The architecture of an Angular application relies on certain fundamental concepts. The basic building blocks of the Angular framework are Angular components that are organized into NgModules. NgModules collect related code into functional sets; an Angular application is defined by a set of NgModules. An application always has at least a root module that enables bootstrapping, and typically has many more feature modules.

Components define views, which are sets of screen elements that Angular can choose among and modify according to your program logic and data

Components use services, which provide specific functionality not directly related to views. Service providers can be injected into components as dependencies, making your code modular, reusable, and efficient.

Modules, components and services are classes that use decorators. These decorators mark their type and provide metadata that tells Angular how to use them.

The metadata for a component class associates it with a template that defines a view. A template combines ordinary HTML with Angular directives and binding markup that allow Angular to modify the HTML before rendering it for display.

The metadata for a service class provides the information Angular needs to make it available to components through dependency injection (DI)

Components

Every Angular application has at least one component, the root component that connects a component hierarchy with the page document object model (DOM). Each component defines a class that contains application data and logic, and is associated with an HTML template that defines a view to be displayed in a target environment.

[![](https://lh4.googleusercontent.com/_TkSxw0dCaA-MGkg8icUdMva04v4rUzz-dQSoUGykjamAGhG9uIwoPKwUB_gx8LenBMMiClrpaKcecHytePNZOYO3eHTRvkSorcgP8qwRioiVxi6TpUrPwpkE6XU8qaHYcXmr_2nCzBMOlsrxQyN1xtMm1ROCGw7QFDFx8v9cBhodyHYsTJQBkRuPWrfyA)](https://lh4.googleusercontent.com/_TkSxw0dCaA-MGkg8icUdMva04v4rUzz-dQSoUGykjamAGhG9uIwoPKwUB_gx8LenBMMiClrpaKcecHytePNZOYO3eHTRvkSorcgP8qwRioiVxi6TpUrPwpkE6XU8qaHYcXmr_2nCzBMOlsrxQyN1xtMm1ROCGw7QFDFx8v9cBhodyHYsTJQBkRuPWrfyA)

[![](https://lh5.googleusercontent.com/Voiw89H8FOMhvGENRNb7xpje0HjbsNfRkOazAXND6DZoqFpOa4RKlrFa0oBQRWk4HYD4KuKEKsQGRgXJIQJEiZL24BGydl2g64OxRp-ulqrKIjtku03ExgfarIMRK5sTspeSnBEioFfYgvhKuFARt-zyGT9_l5K0xeCyxJEkVBh4kpaZlnb4R1Mmjak5Sw)](https://lh5.googleusercontent.com/Voiw89H8FOMhvGENRNb7xpje0HjbsNfRkOazAXND6DZoqFpOa4RKlrFa0oBQRWk4HYD4KuKEKsQGRgXJIQJEiZL24BGydl2g64OxRp-ulqrKIjtku03ExgfarIMRK5sTspeSnBEioFfYgvhKuFARt-zyGT9_l5K0xeCyxJEkVBh4kpaZlnb4R1Mmjak5Sw)

The @Component() decorator identifies the class immediately below it as a component, and provides the template and related component-specific metadata.

Decorators are functions that modify JavaScript classes. Angular defines a number of decorators that attach specific kinds of metadata to classes, so that the system knows what those classes mean and how they should work.

## **Directives and pipes**

[![](https://lh5.googleusercontent.com/OAEcgGk4UsJq5B9YbAK_lPWHKwlxAor_rqXg2qokRJ2boYuv66mkF40eBLN0sEcJWA8clMOIg2vFAs23M0vqDPzJ-ze7-EWiTEMDcJE3fYfJBpZ4n3Mhh_9cYfmFHtCk8p8rY7KrWxWVXFHVB_0A-zllGhxyOQ3prLW_Cc1VK5CX9BWK6kavWg4PMnYB8Q)](https://lh5.googleusercontent.com/OAEcgGk4UsJq5B9YbAK_lPWHKwlxAor_rqXg2qokRJ2boYuv66mkF40eBLN0sEcJWA8clMOIg2vFAs23M0vqDPzJ-ze7-EWiTEMDcJE3fYfJBpZ4n3Mhh_9cYfmFHtCk8p8rY7KrWxWVXFHVB_0A-zllGhxyOQ3prLW_Cc1VK5CX9BWK6kavWg4PMnYB8Q)

## **Data binding**

[![](https://lh5.googleusercontent.com/vf_9W-UfJx5gSjF_NuOHZK-MXOx9Y_VIz9UIMrR3cwPa1Rn7PoSRaaBoltnOsuPBd1O3KqQ79Pu3UxZJkXzZAdt5Tm4VaLozEqWcBk5rMB1poS-lroldljn6GyPiVa1EPzepZmaX7s08iUPV46Eg-gwfr3jA2AjKlXm8X4TMhZWBegrISLb3itzthTQOyQ)](https://lh5.googleusercontent.com/vf_9W-UfJx5gSjF_NuOHZK-MXOx9Y_VIz9UIMrR3cwPa1Rn7PoSRaaBoltnOsuPBd1O3KqQ79Pu3UxZJkXzZAdt5Tm4VaLozEqWcBk5rMB1poS-lroldljn6GyPiVa1EPzepZmaX7s08iUPV46Eg-gwfr3jA2AjKlXm8X4TMhZWBegrISLb3itzthTQOyQ)

## **Dependency injection**

## **Data persistence**

[![](https://lh6.googleusercontent.com/4ZugYTfLsoJ6TxYLJSB04CF1FG-fAvAaQthhvdaSZ1KEnqbI43wTcGCHKyOXJYHjN3dq_XCPmS52ZEamdLiZCa7H1N3_XpNfvgnFjNZfg3hFkykrKabc32ObtQsusIdOcvZp9bQCCndxi6cRiXgoPPF4Ie3Fxy-iLFFm1oj2T2PZUWmdzOMwsbJcMtUQQQ)](https://lh6.googleusercontent.com/4ZugYTfLsoJ6TxYLJSB04CF1FG-fAvAaQthhvdaSZ1KEnqbI43wTcGCHKyOXJYHjN3dq_XCPmS52ZEamdLiZCa7H1N3_XpNfvgnFjNZfg3hFkykrKabc32ObtQsusIdOcvZp9bQCCndxi6cRiXgoPPF4Ie3Fxy-iLFFm1oj2T2PZUWmdzOMwsbJcMtUQQQ)

[![](https://lh4.googleusercontent.com/YrpYUuYSyAaLVp7tIK-dJjYcaVg3kaVr5zUAPOHBrp898yvzBGlxIkrxPfAtaAYvOMYt3WZHKs8iKGCLPsvHKNC_Xj7X0H1Z6_6pEUhPj3fybl-CDVjEFvRjReHxlNo-6Xx4pRrslURGwG7N81gc4YJTjIp3Yl4V9viLC_Iu0_Az3NmlIgA_i-JkmF1e2Q)](https://lh4.googleusercontent.com/YrpYUuYSyAaLVp7tIK-dJjYcaVg3kaVr5zUAPOHBrp898yvzBGlxIkrxPfAtaAYvOMYt3WZHKs8iKGCLPsvHKNC_Xj7X0H1Z6_6pEUhPj3fybl-CDVjEFvRjReHxlNo-6Xx4pRrslURGwG7N81gc4YJTjIp3Yl4V9viLC_Iu0_Az3NmlIgA_i-JkmF1e2Q)

[![](https://lh5.googleusercontent.com/nnXaXIr-C2EaCxdCEMuCeeUi95fMvjeQZPRWnGMzKId3DvKEoCWBZ96MbEk5QiwJXHDe8wtvqwJQuNIfWLZWo0HHhm27-JZmgeIBgXGD5WfsQWKt9E0jeZolHIHn_DNmEvR7uXfiOeeH2MbJV0glChztDGZZBGmZGurjb0AzAHJnnc0Pjgdgy7vgMAMlGQ)](https://lh5.googleusercontent.com/nnXaXIr-C2EaCxdCEMuCeeUi95fMvjeQZPRWnGMzKId3DvKEoCWBZ96MbEk5QiwJXHDe8wtvqwJQuNIfWLZWo0HHhm27-JZmgeIBgXGD5WfsQWKt9E0jeZolHIHn_DNmEvR7uXfiOeeH2MbJV0glChztDGZZBGmZGurjb0AzAHJnnc0Pjgdgy7vgMAMlGQ)

**Routing**

[![](https://lh4.googleusercontent.com/DgA2ans3H9vcnuiBO055gvOgMoZUAqgxSarWRs4Am5StdQgbAq3Iywya5rIDPfZMKyQQ64V-xW0Y09PBRqHR-jRGVHD8rWLUvHrvjcfd8MsT-OVyqnC3PDez8Ax7S3qYnVqSkZr9gquYP69Xpm9hcIJ8Ex1opAdZ-bFbzFk9vp4Cyduyu5dvk6DRN-nFzQ)](https://lh4.googleusercontent.com/DgA2ans3H9vcnuiBO055gvOgMoZUAqgxSarWRs4Am5StdQgbAq3Iywya5rIDPfZMKyQQ64V-xW0Y09PBRqHR-jRGVHD8rWLUvHrvjcfd8MsT-OVyqnC3PDez8Ax7S3qYnVqSkZr9gquYP69Xpm9hcIJ8Ex1opAdZ-bFbzFk9vp4Cyduyu5dvk6DRN-nFzQ)

**NgModule**

[![](https://lh6.googleusercontent.com/sN-lh9bhzlC1FH5Ypgj6B5oNUJofGbYn-444HtBnvf4_6IpZ4KzCRczHhW9u6xW96R4C_GXTQmzV7KCag63dYetYm1OL657_8-cNCIUamxkpTTDEDgeYgpkH7OiOnaElIMjDQoZVT-c01qcqL6UZdVZJXxUjIU4-FHkSf49mcZe47_FxBjJX7_xC4hhEig)](https://lh6.googleusercontent.com/sN-lh9bhzlC1FH5Ypgj6B5oNUJofGbYn-444HtBnvf4_6IpZ4KzCRczHhW9u6xW96R4C_GXTQmzV7KCag63dYetYm1OL657_8-cNCIUamxkpTTDEDgeYgpkH7OiOnaElIMjDQoZVT-c01qcqL6UZdVZJXxUjIU4-FHkSf49mcZe47_FxBjJX7_xC4hhEig)
[![](https://lh3.googleusercontent.com/6nFeDFU4LnvBGpXeMddJCunDT98tOdJklg5L_fuwhibd0Ysp6doP45YWo4sLOZ2R457G05RfLZOcMUfAaFEmO77Tuv1jz0LHzWdeey6MfOMyPfpECYh8TOhToSAsERWuEDEyUpspdxIk7KWULY3wVCwwj-NKXX2jT1pxeHKcEjtFMZ_P7YhFDrOsgdv3NQ)](https://lh3.googleusercontent.com/6nFeDFU4LnvBGpXeMddJCunDT98tOdJklg5L_fuwhibd0Ysp6doP45YWo4sLOZ2R457G05RfLZOcMUfAaFEmO77Tuv1jz0LHzWdeey6MfOMyPfpECYh8TOhToSAsERWuEDEyUpspdxIk7KWULY3wVCwwj-NKXX2jT1pxeHKcEjtFMZ_P7YhFDrOsgdv3NQ)