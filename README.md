# Technical Interview
Welcome to the OutStem Summer 2022 Full Time technical interview

## Introduction
In this take-home assessment, we will look at your ability to design and implement a human friendly, responsive user interface and interact with back-end endpoints.

For this take-home assessment, you shall design and implement a front-end authentication flow UI that will safely handle the user's account details and respond to a variety of authentication challenges.

You will be given a few mock server endpoints that will shall integrate in your application. Unfortunately, the back-end developers are all on vacation so you are required to understand the endpoints by looking at the references the developers wrote before they left.

_All scenarios in this assessment are fictional and are not reflective of the experience you will have during your day-to-day work._

## The Challenge
Recently, the business team proposed a unified identity authentication interface (let's call it AuthApp for now) for all of OutStem's applications. All of OutStem's apps will redirect the user to AuthApp when authentication is required.

The sign in flow you will implement has 3 pages:
- Sign in options page
- Sign in with email and password page
- MFA code page

## Pages
### Sign in options page

Our business team would prefer users to prioritize Social Sign In options over logging in with an email and password, this reduces the risk of fraud and simplifies the sign in experience for our users. They can click on a button and reuse their existing identities with Google or Microsoft for our app.

The page will have three buttons: 
- Sign in with Microsoft
- Sign in with Google
- Sign in with Email

**_For this assessment, sign in with Microsoft and Google does not need to be implemented, the presence of the buttons is enough._**

The sign in with email button will take the user to the sign in with email and password page.

### Sign in with email and password page

On this page, the user will enter an email and a password to sign in. The page should ensure the email and password are present and the email is in valid email format before sending the request to the server.

When the user submits their credentials three things may happen:

1. The credential is valid, the user is redirected to where they need to go.
1. The credential is valid but the user set up an additional MFA challenge, the user is taken to the MFA page.
1. The credential is invalid or an error occurred, an error message should be displayed.

### MFA page

Some users who have accounts with us also set up MFA to boost their accounts' security. Our identity system only supports MFA with an authenticator app. If the identity API indicates that the user has setup MFA challenge, this page is shown so the user can enter a code from the authenticator app on their personal devices.

## Redirection

One of the key features of the Auth app is that it allows the users to redirect back after the authentication is completed. Therefore, your AuthApp should be accessible with a query param set in the URL:

`https://your-assessment.com/?redirect=http://interview.outstem.io/oauth`

When the user completes the sign in, your app shall redirect the user to the following link:

`http://interview.outstem.io/oauth?identity=SOME_IDENTITY_STRING`

## The Auth API endpoints

### Initiate auth
To initiate authentication, call the initiate auth endpoint.

`POST http://interview.outstem.io/auth`

The endpoint accepts the following body:

```
interface AuthBody {
  email: string;
  password: string;
}
```

The endpoint will respond with the following body:

```
interface AuthResponse {
  challenge?: 'MFA' | null,
  identity?: string | null
}
```

If the user has MFA setup, the challenge will be 'MFA' and the identity string will be null or undefined. If the user does not have MFA setup, it's vice versa.

The endpoint may return the following error responses:

```
interface AuthError {
  code: 400 | 500;
  message: string;
}
```

The HTTP status code will also be correctly set to reflect the status code in the body.

### Verify MFA code

To verify the user's MFA code use the verify mfa endpoint

`POST http://interview.outstem.io/auth/mfa`

The endpoint expects the following body:
```
interface MfaBody {
  code: string;
}
```

The endpoint will return the following response if code is correct
```
interface MfaResponse {
  identity: string;
}
```

The endpoint will throw the following error if the code is invalid
```
interface MfaError {
  code: 400 | 500;
  message: string;
}
```

The HTTP status code is also correctly set for the response

## Testing
To test authentication functionalities, you can use the following usernames:
- mfa@outstem.io - requires MFA challenge
- user@outstem.io - no MFA challenge
- Every other username will result in a 400.

All test users have the same password - `testtest`

The valid MFA code is `123456`

## Requirements, evaluation criteria, and bonus challenges
### Requirements
-   Implement the assessment in TypeScript/JavaScript (Preferably Typescript)
-   Use one of the following UI frameworks (Preferably Angular):
    -   Angular
    -   React
    -   Vue
    -   Svelte
-   The AuthApp must read a redirect URL from the URL query param. If the URL is not present, display an error to the users.
-   The UI should be clean, modern, and user-friendly
-   The UI must be responsive, that is usable of all screen sizes. Preferably developed in a mobile first manner.

###  Evaluation Criteria
-   Completeness: did you complete the features?
-   Correctness: does the functionality act in sensible, thought-out ways?
-   Maintainability: is it written in a clean, maintainable way?
-   Testing: is the system adequately tested?
-   Best Practices: Use of Javascript/TypeScript and your chosen framework's best practices
-   Version Control: Work is shown through your commit history

### Bonus Challenges
-   Implement a password reset UX flow
-   Create account page which collects
    -   Email address
    -   First and last name
    -   Password (8 characters minimum)
-   Find other ways to improve the overall UX

## Submitting
To submit your take home assessment, create a new issue in this repository and fill out the template provided.

We would like to emphasize that our goal is to evaluate your technical proficiency, and that the challenge is part of our discussion with you during the interview, so please submit as much of the challenge as you manage to complete
