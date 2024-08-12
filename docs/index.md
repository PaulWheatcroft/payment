# Payment Solution

## Introduction
An outline and overview of the options and design patterns that could be used in a payment app built using JavaScript and React

## Architecture

### Data Flow

#### Handling Everything in the Browser

![browser data flow](browser-data-flow.svg)

<div style="background-color: #c5ffc8;">
**Pros**
- <span style="background-color: #c5ffc8;">Simple</span>
- <span style="background-color: #c5ffc8;">Quick - *direct communication with payment server*</span>
- <span style="background-color: #c5ffc8;">Cheaper - *no backend infrastructure required*</span>
- <span style="background-color: #c5ffc8;">Easier - to deploy *there are less components*</span>
</div>

**Cons**
- <span style="background-color: #ffcfc5;">Security - *handling things in the browser is less secure*</span>
- <span style="background-color: #ffcfc5;">Less app behavioral control - *error handling is down to the payment provider*</span>
- <span style="background-color: #ffcfc5;">Compliance issues - *due to the loss of security as above*</span>
- <span style="background-color: #ffcfc5;">Scalability - *adding additional functionality might be more cumbersome*</span>

#### Handling Stripe via Backend Service

![backend data flow](backend-data-flow.svg)

**Pros**
- <span style="background-color: #c5ffc8;">Better security - *less data is exposed as this is kept server side*</span>
- <span style="background-color: #c5ffc8;">Better app behavioral control - *errors can be more effectively controlled. Integration with other backend services (own email service?)*</span>
- <span style="background-color: #c5ffc8;">Compliance - *easier to get compliance as is more secure*</span>
- <span style="background-color: #c5ffc8;">Scalability - *would be easier to add functionality to the payment flow as the logic is primarily handled server side*</span>

**Cons**
- <span style="background-color: #ffcfc5;">More complex</span>
- <span style="background-color: #ffcfc5;">Slower - *payment now has a second step of complexity to negotiate*</span>
- <span style="background-color: #ffcfc5;">More expensive - *as there is more infrastructure*</span>
- <span style="background-color: #ffcfc5;">More to manage - *there is more to deploy and maintain*</span>

Feels like integrating with a backend solution to manage the payments is a good idea. Also transaction can be store in an ACID database such as PostgreSQL to ensure the integrity of the data.

## Payment Service

Create a payment service class that can be extended for different payment services. This creates an interface that abstracts out the specific payment provider logic. This can be dynamically loaded to use the correct prover via env variable i.e. env.PAYMENT_PROVIDER

```
class PaymentService {
  initialisePaymentOnPaymentProvider(amount, currency) {
  }

  processPaymentOnTheBackend(token) {
  }

  handleWebhookFromPaymentProvider(event) {
  }
}
```

```
class StripePaymentService extends PaymentService {
    await initialisePaymentOnPaymentProvider(amount, currency) {
    }

    await processPaymentOnTheBackend(token) {
    }

    handleWebhookFromPaymentProvider(event) {
    }
}
```

```
class SomeOtherPaymentService extends PaymentService {
    await initialisePaymentOnPaymentProvider(amount, currency) {
    }

    await processPaymentOnTheBackend(token) {
    }

    handleWebhookFromPaymentProvider(event) {
    }
}
```

> Payment services provide their own client side library for capturing credit card details
> - frontend sends the payment details to the payment service
> - a payment token is received
> - this token is sent to the backend
> - backend processes the payment
> - payment provider will return webhook events
> - status sent back to the frontend

## Context and State

Rather than passing props from one component to another wrap the "App" in a react createContext

```
const App = () => {
    return (
        <FormProvider>
            <CheckoutForm />
        </FormProvider>
    );
};
```

The context provider contains the useState to manage the form events and the payment status.

An alternative would be to implement a state management solution such as Redux. 

```
const App = () => {
    return (
        <Provider store={store}>
            <Counter />
        </Provider>
    );
};
```

I'm not sure this app is complex enough to warrant the additional complexity