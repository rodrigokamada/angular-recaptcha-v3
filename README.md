# Angular Google reCAPTCHA v3


Application example built with [Angular](https://angular.io/) 15 and adding the Google reCAPTCHA v3 using the [ng-recaptcha](https://www.npmjs.com/package/ng-recaptcha) library.

This tutorial was posted on my [blog](https://rodrigo.kamada.com.br/blog/adicionando-o-google-recaptcha-v3-em-uma-aplicacao-angular) in portuguese and on the [DEV Community](https://dev.to/rodrigokamada/adding-the-google-recaptcha-v3-to-an-angular-application-kge) in english.



[![Website](https://shields.braskam.com/v1/shields?name=website&format=rectangle&size=small&radius=5)](https://rodrigo.kamada.com.br)
[![LinkedIn](https://shields.braskam.com/v1/shields?name=linkedin&format=rectangle&size=small&radius=5)](https://www.linkedin.com/in/rodrigokamada)
[![Twitter](https://shields.braskam.com/v1/shields?name=twitter&format=rectangle&size=small&radius=5&socialAccount=rodrigokamada)](https://twitter.com/rodrigokamada)
[![Instagram](https://shields.braskam.com/v1/shields?name=instagram&format=rectangle&size=small&radius=5)](https://www.instagram.com/rodrigokamada)



## Prerequisites


Before you start, you need to install and configure the tools:

* [git](https://git-scm.com/)
* [Node.js and npm](https://nodejs.org/)
* [Angular CLI](https://angular.io/cli)
* IDE (e.g. [Visual Studio Code](https://code.visualstudio.com/))



## Getting started


### Create and configure the account on the Google reCAPTCHA


**1.** Let's create the account. Access the site [https://www.google.com/recaptcha/](https://www.google.com/recaptcha/) and click on the button *v3 Admin Console*.

![Google reCAPTCHA - Home page](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966510/Blog/angular-recaptcha-v3/recaptcha-step1.png)

**2.** Fill in the field *Email or phone* and click on the button *Next* to login with your Google account and if you don't have an account, just create a new account.

![Google reCAPTCHA - Sign up](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966510/Blog/angular-recaptcha-v3/recaptcha-step2.png)

**3.** Click on the button *+*.

![Google reCAPTCHA - Create new](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966510/Blog/angular-recaptcha-v3/recaptcha-step3.png)

**4.** Fill in the field *Label*, click on the option *reCAPTCHA 3*, Fill in the field *Domains*, click on the checkbox *Accept the reCAPTCHA Terms of Service* and click on the button *Submit*.

![Google reCAPTCHA - Register a new site](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966510/Blog/angular-recaptcha-v3/recaptcha-step4.png)

**5.** Click on the button *COPY SITE KEY* to copy the key, in my case, the key `6Lf7UL0cAAAAAIt_m-d24WG4mA1XFPHE8yVckc5S` was copied because this key will be configured in the Angular application.

![Google reCAPTCHA - Adding reCAPTCHA to your site](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966510/Blog/angular-recaptcha-v3/recaptcha-step5.png)

**6.** Ready! The keys have been generated.


### Create the Angular application


**1.** Let's create the application with the Angular base structure using the `@angular/cli` with the route file and the SCSS style format.

```powershell
ng new angular-recaptcha-v3
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
CREATE angular-recaptcha-v3/README.md (1064 bytes)
CREATE angular-recaptcha-v3/.editorconfig (274 bytes)
CREATE angular-recaptcha-v3/.gitignore (604 bytes)
CREATE angular-recaptcha-v3/angular.json (3291 bytes)
CREATE angular-recaptcha-v3/package.json (1082 bytes)
CREATE angular-recaptcha-v3/tsconfig.json (783 bytes)
CREATE angular-recaptcha-v3/.browserslistrc (703 bytes)
CREATE angular-recaptcha-v3/karma.conf.js (1437 bytes)
CREATE angular-recaptcha-v3/tsconfig.app.json (287 bytes)
CREATE angular-recaptcha-v3/tsconfig.spec.json (333 bytes)
CREATE angular-recaptcha-v3/src/favicon.ico (948 bytes)
CREATE angular-recaptcha-v3/src/index.html (304 bytes)
CREATE angular-recaptcha-v3/src/main.ts (372 bytes)
CREATE angular-recaptcha-v3/src/polyfills.ts (2820 bytes)
CREATE angular-recaptcha-v3/src/styles.scss (80 bytes)
CREATE angular-recaptcha-v3/src/test.ts (788 bytes)
CREATE angular-recaptcha-v3/src/assets/.gitkeep (0 bytes)
CREATE angular-recaptcha-v3/src/environments/environment.prod.ts (51 bytes)
CREATE angular-recaptcha-v3/src/environments/environment.ts (658 bytes)
CREATE angular-recaptcha-v3/src/app/app-routing.module.ts (245 bytes)
CREATE angular-recaptcha-v3/src/app/app.module.ts (393 bytes)
CREATE angular-recaptcha-v3/src/app/app.component.scss (0 bytes)
CREATE angular-recaptcha-v3/src/app/app.component.html (24617 bytes)
CREATE angular-recaptcha-v3/src/app/app.component.spec.ts (1115 bytes)
CREATE angular-recaptcha-v3/src/app/app.component.ts (225 bytes)
✔ Packages installed successfully.
    Successfully initialized git.
```

**2.** Install and configure the Bootstrap CSS framework. Do steps 2 and 3 of the post *[Adding the Bootstrap CSS framework to an Angular application](https://github.com/rodrigokamada/angular-bootstrap)*.

**3.** Configure the `siteKey` variable with the Google reCAPTCHA key in the `src/environments/environment.ts` and `src/environments/environment.prod.ts` files as below.

```typescript
recaptcha: {
  siteKey: '6Lf7UL0cAAAAAIt_m-d24WG4mA1XFPHE8yVckc5S',
},
```

**4.** Install the `ng-recaptcha` library.

```powershell
npm install ng-recaptcha
```

**5.** Import the `FormsModule`, `RecaptchaV3Module` modules. Configure the Google reCAPTCHA key. Change the `app.module.ts` file and add the lines as below.

```typescript
import { FormsModule } from '@angular/forms';
import { RECAPTCHA_V3_SITE_KEY, RecaptchaV3Module } from 'ng-recaptcha';

import { environment } from '../environments/environment';

imports: [
  BrowserModule,
  FormsModule,
  RecaptchaV3Module,
  AppRoutingModule,
],
providers: [
  {
    provide: RECAPTCHA_V3_SITE_KEY,
    useValue: environment.recaptcha.siteKey,
  },
],
```

**6.** Remove the contents of the `AppComponent` class from the `src/app/app.component.ts` file. Import the `NgForm` component, the `ReCaptchaV3Service` service and create the `send` method as below.

```typescript
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';
import { ReCaptchaV3Service } from 'ng-recaptcha';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {

  constructor(private recaptchaV3Service: ReCaptchaV3Service) {
  }

  public send(form: NgForm): void {
    if (form.invalid) {
      for (const control of Object.keys(form.controls)) {
        form.controls[control].markAsTouched();
      }
      return;
    }

    this.recaptchaV3Service.execute('importantAction')
    .subscribe((token: string) => {
      console.debug(`Token [${token}] generated`);
    });
  }

}
```

**7.** Remove the contents of the `src/app/app.component.html` file. Add the `re-captcha` component as below.

```html
<div class="container-fluid py-3">
  <h1>Angular reCAPTCHA v3</h1>

  <form #form="ngForm">
    <div class="row mt-3">
      <div class="col-sm-12 mb-2">
        <button type="button" class="btn btn-sm btn-primary" (click)="send(form)">Send</button>
      </div>
    </div>
  </form>
</div>
```

**8.** Run the application with the command below.

```powershell
npm start

> angular-recaptcha-v3@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files | Names         |      Size
vendor.js           | vendor        |   2.75 MB
styles.css          | styles        | 266.71 kB
polyfills.js        | polyfills     | 128.52 kB
scripts.js          | scripts       |  76.33 kB
main.js             | main          |  12.28 kB
runtime.js          | runtime       |   6.64 kB

                    | Initial Total |   3.23 MB

Build at: 2021-10-09T22:00:31.213Z - Hash: f91dc9237b57212ebd83 - Time: 12001ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

**9.** Ready! Access the URL `http://localhost:4200/` and check if the application is working. See the application working on [GitHub Pages](https://rodrigokamada.github.io/angular-recaptcha-v3/) and [Stackblitz](https://stackblitz.com/edit/angular15-recaptcha-v3).

![Angular Google reCAPTCHA v3](https://res.cloudinary.com/rodrigokamada/image/upload/v1633966502/Blog/angular-recaptcha-v3/angular-recaptcha-v3.png)



## Cloning the application

**1.** Clone the repository.

```powershell
git clone git@github.com:rodrigokamada/angular-recaptcha-v3.git
```

**2.** Install the dependencies.

```powershell
npm ci
```

**3.** Run the application.

```powershell
npm start
```
