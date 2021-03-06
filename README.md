## Portmone Integration Guide

Portmone is a service that allows user to share the proven identity facts with 3rd parties without the need to share any of the government issued documents.

This documentation provides an information about the way the integration is implemented and reveals it from the Partner perspective.

Thus the integration includes:

1. Partner authentication (issue of authentication token)
2. Data share request generation and presenting it in form of QR code to the Customer
3. Getting the event of data share request acceptance by the Customer & pulling the shared facts

The sequence diagram below sheds a light on integration details. Let's stop to inspect it.

![diagram-sequence](./assets/diagram-sequence.png)

### Customer User Verification

Everything starts with the customer that is willing to use Partner's service (either financial or restricted by age) that requires the verification of the facts related to the customer's identity.

Thus the Customer User goes to the Partner's page, where he is then presented with Portmone QR code to be scanned and approved. There is the React SDK available to the Partner that simplifies the rendering of the QR code based on the data share request generated by Partner (see the usage following the [link](https://www.npmjs.com/package/@portmone-ai/sdk-react))

### Creating the Data Share Request

Before the Portmone QR code can be shown to the Customer User, that code must be received from Portmone Service.

The process of the Portmone QR code retrieval (data share request generation) must satisfy the security constraints in order to prevent the authentication information to be exposed.

#### Partner's Backend authenticates as a Partner User within Portmone Service

In order for Partner to generate the Data Share Request, Partner must authenticate within Portmone Service by using the credentials (provided by Portmone earlier). Once the authentication token is generated it can be used to call the resources with the restricted access.

Resource dedicated to generated the Data Share Request is restricted and requires the called by be authenticated.

#### Partner's Frontend doesn't generate data share request directly

Once Partner is authenticated the Data Share Request can be generated. Of course, from the security perspective such a call can't be done from Partner's frontend, as it would reveal the authentication token, which must be kept in secret.

#### Portmone QR code is rendered by Portmone SDK (React)

After the Portmone QR code generation, the Partner's backend delivers it to the Partner's frontend to be presented to the Customer User.

In order for the Partner not to implement the rendering mechanisms and the status synchronization mechanism it is highly recommended to use Portmone SDK that implements such a logic, thus simplifies the integration.

### Getting the Identity Facts shared

As soon as the Data Share Request is approved by the Customer User and Portmone SDK finds this out, then the Partner is able to pull the identity facts shared from the Portmone Service.

#### Partner's Frontend doesn't access the Data Share Request related facts

It's important to pay attention that data shared is not allowed to be pulled from Partner's frontend in order not to reveal the authentication related information.